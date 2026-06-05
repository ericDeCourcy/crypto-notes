- Private dex idea
	- groups of users register a private intent together, with private token inputs - ring sigs
	- as private intent gets built, eventually it executes. perhaps a fixed number or random number of queued intents will execute together. Private results will be published together.
		- input: {command, params, receipt-key} -> receipt: {encrypted id of receipt, encrypted with receipt-key}
	- perhaps over some regular interval, 10% of transactions get executed. Or... they're assigned in random order
		- maybe they are ether added to orderbook or cancel out something else in orderbook. All key images to not leak which ones are getting executed
	- price is never revealed outward;y
	- why not just have an amm that has options for execute or nah

## Other implementations
- [[Lunarys]] already exists, but i'm having trouble getting the source code for whats on chain

# What kind of private?
I think users want two things:
- They want their trading pairs to be hidden
- They want the amounts of their trades to be hidden

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

### Processing rounds
I think to prevent timing related attacks, like front running etc, and to prevent trading to learn the pricing or reserve info in real time, we should add delays. Imagine 10 rounds of "processing" required, which are at least 1 block long, such that people can't quickly execute a transaction and learn the pool's state