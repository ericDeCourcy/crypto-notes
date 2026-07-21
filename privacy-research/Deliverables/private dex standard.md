- Private dex idea
	- groups of users register a private intent together, with private token inputs - ring sigs
	- as private intent gets built, eventually it executes. perhaps a fixed number or random number of queued intents will execute together. Private results will be published together.
		- input: {command, params, receipt-key} -> receipt: {encrypted id of receipt, encrypted with receipt-key}
	- perhaps over some regular interval, 10% of transactions get executed. Or... they're assigned in random order
		- maybe they are ether added to orderbook or cancel out something else in orderbook. All key images to not leak which ones are getting executed
	- price is never revealed outward;y
	- why not just have an amm that has options for execute or nah

## Other implementations and previous work
- [[Lunarys]] already exists, but i'm having trouble getting the source code for whats on chain
- This paper ["a note on privacy in constant function market makers"](https://arxiv.org/pdf/2103.01193)
	- This paper makes the claim that mathematically, we cannot keep a shared AMM state totally private, because the outcomes of swaps and stuff will allow us to infer information

# What kind of private?
I think users want two things:
- They want their trading pairs to be hidden
- They want the amounts of their trades to be hidden
Another thing which users/Zama may want
- The dex is transacting on ERC7984's, not ERC20s

# Technical limitations
### Division
Lots of dex designs include some sort of division. This is really difficult to do without leaking information in an FHE system. FHE doesn't allow division without knowing the denominator, which means the denominator goes into the clear space.

Mutliplicative inverses can sometimes be computed but those also may require clear-spacing information to generate. Or there could be a giant ass table but thats really hard to generate

### Linked handles
The memory layout of a dex may reveal what coins are being transacted with. A workaround for this is **decoy transactions** but those may be costly 

### HCU complexity limits
We can't have the design be overly complex

### Revealing information
Revealing pricing information, revealing LP amounts or trade sizes, etc may all leak prices or user trade info which we don't want to do

### Function signature differences
Knowing if a user is depositing, withdrawing, swapping x-for-y or y-for-x gives us a lot of infor in tracing their balances

# Solutions to technical limitations
### Decoy transactions
Basically just sending off a bunch of transactions which do nothing, along with your intended transaction. This is a good way to obfuscate the link between transactions. Needs to be managed well to prevent statistical analysis of the links between decoy sets.

The benefit is that this is a cheap way to add noise to the dataset. Downside is letting users choose may result in low-quality decoy selection, letting provider decide or client decide may leak privacy. Still, client is probably best. 

**Decoy selection** becomes a challenge here

### Sender obfuscation
We could implement some kind of signature mechanism but that might be really difficult, esp with user's balances being represented by diff handles
### Ticks
Instead of implementing a uniswap v2 style dex, implement a v3 style. 

This keeps price fixed, and we can avoid having to div like in the v2 style of transaction.

Downside is that price tick needs to be known ahead of time. Making what tick the price is currently in public makes it difficult to hide price information. But this is okay. It also makes it easier to hide transactions which change the price of the pool. 

#TODO does it matter if this information is leaked? Is there any way to avoid leaking the ticks you are interacting with?

#TODO how does this work for the user flow? 
- they might need to make multiple transactions to do a trade?

#TODO how could this handle the tick changing in the middle of a trade?
- Convoluted but, each request could be for a range of ticks, and any "remainder" after processing gets moved to the next request.

#TODO if ticks are larger, how does it impact prices? Does it obscure information enough to make it difficult to detect large moves in liquidity?

### Processing rounds
I think to prevent timing related attacks, like front running etc, and to prevent trading to learn the pricing or reserve info in real time, we should add delays. Imagine 10 rounds of "processing" required, which are at least 1 block long, such that people can't quickly execute a transaction and learn the pool's state

# Designs
## Individual Tick markets with obfuscation
General idea is to have a bunch of different ticks as "markets" which can be interacted with. Each tick has a price and an A/B token amount. There should be a registry of all pairs and their associated list of ticks. Not all ticks must exist, but we need a doubly-linked list to find the next tick.

**Obfuscation** means that we need decoy transaction. In monero fashion, we can do 16 total "transactions". More than one decoy *can be* real. To make the decoy real or not, modify the amount to be nonzero or not.

Finalization - there should be a delay between executing and seeing outcome of the transaction to prevent instant knowledge from being leaked.

We can use a finalized flag, accessible by the owner of the order.

### Active ticks
The tick is active if A or B amount is nonzero, but is not necessarily inactive if the amount is nonzero. There should only be one active market per coin.

A doubly-linked list allows the market to know where the active tick is. For all ticks which exist, we should have a cleartext pointer to that location. We need a unique identifier for each tick and to be able to see if it already exists.

#TODO how does tickmath work?

### Finalization flag
A user can set access to a flag that shows the status of their order, for re-tries. An order must be tried once before the market can move on. But after this, it can be cancelled once its time period ends.

### Privacy in this design
This design attempts to protect which tokens a user is transacting with and the size of those interactions. It doesn't hide these, it just obfuscates these actions. With enough actions, there should be sufficient obfuscation.

### User experience
might kinda suck... users may not be able to know if their trades have executed for a while. They may get frustrated with having to re-try transactions

### FHE in this design
The math is aimed at being extremely simplistic to avoid FHE complexity and computing fractions/ratios etc

## Mixing periods with UTXOs and stealth addresses
- Every trade is a "U"TXO 
	- Includes token amount and trade intention (trades and LP deposits follow same flow)
	- The token amount is the UTXO. Once you publish an intention for it, it becomes "potentially spent", but not necessarily fully spent
		- Every change in a UTXO must generate a new UTXO and decrement the old one's balance
		- This means we can partially spend UTXOs and then spend the rest of them later
			- This means the amounts *change* and we need to be really careful about double-spends
			- In general, always subtracting and adding balances makes the most sense
- Tokens are not transferred once in the dex. They stay in a treasury contract
	- Instead, UTXO amounts also specify the identifier for the token
- When UTXOs are "partially" spent (we should always consider them partial spends because we don't know if they were fully spent), they assign a recipient **stealth address**. A new, unused address which the owner can sign from.
- mixing - since each spend of a UTXO is indistinguishable, we can optionally swap two of them,
	- This means swapping all information in the handles used for both UTXOs.
	- If we use zama randomness, we can swap both if a bit is 1, or do nothing if its 0
	- We can require mixing up to 5 times (for example) or with 5 unique UTXOs, before and after executing trades
		- mixing before trades makes it hard to identify who triggered the trade.
			- May need to consider the architecture here #TODO because ideally it should de-link the input UTXO
		- mixing after the trade does two things
			- delay prevents information about trade execution from getting leaked for a bit. We have to delay the outcome of trades so that users cannot gather too much info about the market
			- it de-links the recipient and the market they were just interacting with.
- At different stages, different information is revealed. For example, the destination for funds should not be revealed until after post-trade mixing