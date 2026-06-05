Backlink: [[Lunarys]]

Reference implementation: https://github.com/6ygb/CAMM

# Design
-  An optional external **price scanner address** is whitelisted to read those obfuscated values facilitating decryption from a front end.
	- For [[Lunarys]], does a price scanner address exist? #TODO 
	- #TODO does such a price scanner reveal private information?
- To get around division issues (not being able to divide by encrypted number), they leverage "division invariance" - the concept of multiplying both sides of a fraction to get an equivalent fraction
- Is the 7% distortion a maximum of the distortion, or a minumum?

# Code notes
*See "reference implementation" above*
- There's a 5 minute decryption delay, which gets reset upon adding liquidity

# User Flows
### Add Liquidity
#TODO 
- There's a callback


# Smells
[[NFP_22]] 

# Privacy Strategies
*"In CAMM, reserves are encrypted. Broadcasting the exact price of a token to the other (by decrypting the price) could potentialy leak :
 -The proportion of a reserve to another (which is not that sensitive)
-The price impact of a swap, potentialy giving an approximative idea of its size"*
- Taken from the github
- Really like this because it details whats important in the dex #insight - trade size is far more important to keep private than reserve sizes
	- Reserve size still matters here because it and trade size are entangled. The price scanner broadcasts the **obfuscated reserves** (so we can see the price without knowing the exact price impact)
-  In order to get around division limitations, there are certain 2-step procedures that need to happen before we can complete actions like depositing liquidity n shit
	- #TODO understand this better
## Information leakage
- Pretty sure the price can leak, because obfuscated reserves are available in the clearspace. See [CAMMPair.sol::L460-461](https://github.com/6ygb/CAMM/blob/149e840a47a23e1d282e612d1534fecb8530fe8f/contracts/CAMMPair.sol#L460-L461) 
	- This is "fine" because it doesn't reveal price-impact of trades

# Backlinks
[[Lunarys]]
