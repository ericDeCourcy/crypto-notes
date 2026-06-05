*This is a dex for ERC7984 tokens*

Backlinks/Obsidian links [[private dex standard]], [[CAMM]]

- Right now they are using FHEnix tokens, not Zama tokens
	- #TODO lets find the contract addresses for these

# Code
- The code onchain is NOT open-sourced. There is similar code:
	- Here's the Dev's [personal github implementation of a CAMM](https://github.com/6ygb/CAMM) 
		- See: [[CAMM]] (obsidian page) 
	- [This repo from different dev (tomi204)](https://github.com/tomi204/privacy-pool-monorepo) is another version
- Currently working on decompiling the sepolia dex: https://app.dedaub.com/sepolia/address/0xf0ab79a82f2d1d5b0230815d26fb0278a237ba3c/overview
# Design
- To mask reserve size, a random scaling amount is applied that displays reserves within 0.7-3.26% of acutal - results in price being within 7% of actual
- Read about it on the [main dev's personal webpage](https://6ygb.dev/)
	- [[6ygb]] - #TODO make this page
- See [[CAMM#Design]]

# Docs
- [Link to lunarys docs](https://docs.lunarys.app/docs/pool-architecture) 

# Deployments
- App is deployed on Ethereum Sepolia and Arbitrum Sepolia
	- It uses FHEnix [[eTokens]] deployed on those networks
- See the [Addresses page from the docs](https://docs.lunarys.app/docs/addresses) 
- Dedaub online decompiler tool for main lunarys contract AFAICT: https://app.dedaub.com/sepolia/address/0xf0ab79a82f2d1d5b0230815d26fb0278a237ba3c/overview

# App
- App itself wasn't working well for me - fairly slow and displayed 1 eUSD -> 0.996 eBTC or 0.996 eETH. Not sure if its just loading or what. I had a lot running in the background so it could be that
	- #TODO retry later

# Links
- https://x.com/LunarysLabs
- https://www.lunarys.app/

# Sepolia Test Transactions
[[NFP_26]] 

# Open Questions
- [ ] I took all these addresses from a signature request from metamask. I think they are related to FHEnix [[eTokens]] - find out what they are. (ETH Sepolia)
```
0x00689E12aE9a2020a2EcdE27e84C4A67A0eCce49
0x25aE630fBdB2EDc6D0053cf93a5e1D9739cBb528
0x288ddB6c9912f849cDF49D995e4AcC0f3902186B
0x5112d35acc479EA1a9B406d1c7B36b37C2A196d7
0x531385Bf03cF41CbfAfc424C67e6A6e9f5B6cad7
0x7F1117204141c6044579C965Ce1fcca1e51E74f2
0x8EFe087A2895D942574DB6764A884612fCe4EBE1
0xE71c606EeEfe8452357fFA03C4B327480C8D361a
0xF7De724CC4ef8c881a5aEbae274A3c05dEF7aa40
0xf0aB79A82F2D1d5b0230815d26fB0278a237bA3c
```
	