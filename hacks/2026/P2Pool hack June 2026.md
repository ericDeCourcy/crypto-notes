### Links
- [Disclosure](https://github.com/SChernykh/p2pool/security/advisories/GHSA-fm6j-gf38-p925)
- [Reddit thread](https://www.reddit.com/r/Monero/comments/1u683ow/p2pool_vulnerability_is_being_actively_exploited/) 


## TL;DR
*copied from disclosure above*

A serious bug was found in P2Pool. **Please upgrade your P2Pool to the latest release and restart it as soon as possible.**

- **Your Monero and your wallet keys are safe.** Nobody can steal your coins or your keys with this bug.
- **Your future payouts can be affected in a negative way, down to nothing** if you keep running an old version while an attacker is active.
- **Old and new versions can end up on two different side-chains** during an attack. Old versions can get "stuck" on the wrong one for a while.
- **The fix is simple: upgrade.** Updated nodes are completely immune.

### Versions
Affected is <4.16,
4.16 is patch version

### Vulnerability
It lets an attacker mine **one** share and then make **thousands of fake copies** of it. Every copy looks valid to old P2Pool versions, even though the attacker only did the work for one.