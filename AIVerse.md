- Simple few clicks to deploy an agent
- At the time of writing (April 29 2026) it costs 0.1 0G (mainnet) tokens to deploy an agent
- [0x949fa60a705124f4b953f5bfc7aac838a12b8b0a](https://explorer.0g.ai/mainnet/blockchain/accounts/0x949fa60a705124f4b953f5bfc7aac838a12b8b0a/verified-contracts)  is the contract address for deploying agents
	- appears that interacting (prompting) with agents doesn't necessarily trigger a transaction on 0G
- #TODO cannot get a "private" iNFT deployed, managed to deploy exactly 1 public agent

### Onchain components
- iNFTs appear to mostly get deployed by this contract 0xd2CDaaAC19695EF960BCf0176C894E7ccE968B19
- https://chainscan.0g.ai/address/0x2ff0f380d85543e0ab6d32eba80da7f3db332dcb
	- This appears to be the address which actually does the agent creation - its logged as the creator for different iNFTs
- Here's an example iNFT - https://chainscan.0g.ai/nft/0xd2CDaaAC19695EF960BCf0176C894E7ccE968B19/125

### Links
- [[iNFT]] 