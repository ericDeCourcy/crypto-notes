Reference implementation: https://github.com/6ygb/CAMM

# Design
-  An optional external **price scanner address** is whitelisted to read those obfuscated values facilitating decryption from a front end.
	- For [[Lunarys]], does a price scanner address exist? #TODO 
	- #TODO does such a price scanner reveal private information?
- To get around division issues (not being able to divide by encrypted number), they leverage "division invariance" - the concept of multiplying both sides of a fraction to get an equivalent fraction

# Smells
[[NFP_22]] 

# Privacy Strategies
*"In CAMM, reserves are encrypted. Broadcasting the exact price of a token to the other (by decrypting the price) could potentialy leak :
 -The proportion of a reserve to another (which is not that sensitive)
-The price impact of a swap, potentialy giving an approximative idea of its size"*
- Taken from the github
- Really like this because it details whats important in the dex #insight - trade size is far more important to keep private than reserve sizes
	- Reserve size still matters here because it and trade size are entangled. The price scanner broadcasts the **obfuscated reserves** (so we can see the price without knowing the exact price impact)

# Backlinks
[[Lunarys]]
