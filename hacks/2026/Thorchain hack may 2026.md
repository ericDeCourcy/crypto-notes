Hack on may 15th, 2026, for a total of $10.8M

# What happened

***May 17 2026***
Discussion so far points to private key theft within threshold signature scheme allowing hacker to drain protocol wallets.

According to [secureshift](https://secureshift.io/blog/thorchain-exploit-analysis), the leading theory is that the attacker was a node operator who participated in signing ceremonies within the GG20 TSS, and each time was able to gain partial private key knowledge. Over time they were able to compute the private key of a vault. 

```
This is not a novel class of vulnerability conceptually. Academic research has identified "key extraction" weaknesses in some multi-party computation (MPC) protocols where a malicious participant can extract bits of other parties' secret shares across multiple protocol rounds. The GG20 paper itself acknowledges the importance of using "identifiable abort" variants to prevent such attacks.
```
*from [secureshift](https://secureshift.io/blog/thorchain-exploit-analysis)*

***May 18 2026***
[Incident response #3 tweet from THORChain](https://x.com/THORChain/status/2056439951697351150?s=20)
- They do NOT believe it to be related to known GG20 attack vectors
- They will release version 3.18.1 for thorchain node operators tomorrow, may 19 2026
	- This is implied to fix the vulnerability
		- #OpenQuestion What does this patch do?
- Discussion still open for best approach about how to handle lost funds
	- Call for community to share proposal
- They Thorchain team is leaning towards continuing to use GG20 instead of introducing more changes

# Incident response
- governance executed `make pause`, which pauses $RUNE transfers for 12 hours, and all trading and LP actions indefinitely.

# Discussion
-  https://x.com/arik_g/status/2055961631423598725?s=20
	 - This is a technical thread on it by the CPO of Zama
	 - "a short list of ECDSA TSS protocol and libraries that should not be in production right now"
	 - Mentions GG18 and GG20 - two Threshold Signature Schemes (TSS)
		 - These are vulnerable to TSSHOCK, which allow key extraction while leaving no trace
		 - [Writeup on TSSHOCK by Verichains](https://verichains.io/tsshock/) 
 

# Sources
1. https://www.coindesk.com/tech/2026/05/15/thorchain-halts-trading-after-usd10-million-cross-chain-exploit-rune-token-drops-12
2. https://secureshift.io/blog/thorchain-exploit-analysis
3. https://therecord.media/more-than-10-million-stolen-crypto-platform-thorchain
4. [Thorsec tweet - "incident update #3"](https://x.com/THORChain/status/2056439951697351150?s=20) 