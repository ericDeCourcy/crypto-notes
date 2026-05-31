# See also
[[Zama cUSDC Frozen may 2026]]

# TLDR
I believe we can return most of the money to its rightful owners without allowing the hacker to abscond w funds

# Contract addresses

| **Address**                                | Name                                                 |
| ------------------------------------------ | ---------------------------------------------------- |
| 0x0309b4308a6ac121b9b3a960ac7bc9bd8256cf38 | OLD cUSDC implementation                             |
| 0x43506849d7c04f9138d1a2050bbf3a0c054402dd | USDC token implementation                            |
| 0x517bd0e1fbe9ddf828fd139b19cf9f434da94371 | Zama multisig implementation                         |
| 0x96b171e4f8ecda0ffdd128b319fa185b14f99d76 | Zama ACL implementation                              |
| 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48 | USDC token                                           |
| 0xB6D69D5F334d8B97B194617B53c6aB62f8681Ef3 | `owner` of cUSDC contract                            |
| 0xbc860b6a4c860c5424b84a056e53acfb2c99a38f | Zama governance(?) multisig (did the upgrade)        |
| 0xca2e8f1f656cd25c01f05d0b243ab1ecd4a8ffb6 | Zama ACL proxy                                       |
| 0xe978f22157048e5db8e5d07971376e86671672b2 | cUSDC contract                                       |
| 0xff9e1db30031cb2b1b82e138bec70d7de70d8413 | cUSDC implementation (upgraded to in block 25077611) |

# ACL blocking

- This transaction is an example of account blocking: https://etherscan.io/tx/0x2e0ccbb28412bd8a02d0829ddcf28ea3526d22e52b052edbdae43df0680ee683#eventlog
	- blocks the `0xda9396b82634Ea99243cE51258B6A5Ae512D4893` (cWETH)
- Other txs also block cUSDT and cUSDC
# Understanding USDC contract freeze mechanism
- USDC contract is proxy
	- [Implementation contract is 0x43506849d7c04f9138d1a2050bbf3a0c054402dd](https://etherscan.io/address/0x43506849d7c04f9138d1a2050bbf3a0c054402dd#code)
- Freezing is called "blacklisting" 
- done by calling public function `blacklist`
	- blackister role held by this address: 0x0A06BE16275B95a7d2567fBdAE118b36C7DA78F9
- Freezing zama cUSDC was done in this transaction: https://etherscan.io/tx/0x6e24a776f7775ada543e52d10ebadf962d91e0b8258eb858c2487754bd199566
- There's also an un-blacklist function
# Understanding the cUSDC contract pause
- [cUSDC implementation address](https://etherscan.io/address/0xff9e1db30031cb2b1b82e138bec70d7de70d8413) 

# Investigations
## How many unique ids are there before the hack and their balances?
- [ ] How many wraps, unwraps and transfers are there excluding the hackers network
	- UNWRAP = 26
	- UNWRAP W PROOF = 176
	- CONF_TRANSFERS = 2
	- CONF_TRANSFER_W_PROOF = 82
	- WRAP = 193
- [ ] Can we write a script to just attempt analysis of every address?
	- [ ] Find every WRAP, find the address doing it and add it to a list. 
		- [ ] Then, put those through analyze-address, but a stripped down version. 
		- [ ] Add all addresses to another list and de-duplicate

## Whats the non-hacker total balance?
`12606386 - 12497334 = 109052`


## Can coinbase mint funds to the affected users? Can they remotely burn the funds?
- minters can mint funds
- I don't think they can burn the funds

## Can partial blacklisting be done?
- don't think so

## Can users finalize their withdrawals to reveal how much they had, then get payment?

## What does it mean that the zama cUSDC contract is frozen? Can they move funds around?
- the cUSDC, cUSDT and cWETH contracts were blocked on the ACL 

## How does being an operator work?
- [ ] 

## Is the contract upgradeable?
Looks like it may very well be