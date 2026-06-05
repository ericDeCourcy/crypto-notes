### TODOs
- [ ] Investigate GDPR requirements
- [ ] Investigate AML and KYC requirements, and how current standards meet them

### Confidential token association
- [Blog: "Inco Launches Confidential Token Association With Open Zeppelin and Zama"](https://www.inco.org/blog/inco-launches-confidential-token-association-with-open-zeppelin-and-zama?utm_source=chatgpt.com)
>The Confidential Token Association’s goal is to develop a framework for empowering token standards such as ERC20 with confidentiality. Leveraging cutting-edge encryption techniques, the confidential token standard allows users to conceal the amounts transferred between addresses, while uniquely preserving the transparency of fund flows onchain.


### `Inco` Confidential Token Standard
[inco.org](https://www.inco.org/)
- "The confidentiality layer of web3"
- FHE based
- Delegated View permissions for regulators
- "Comfy" - Tool for private tokens [comfy.inco.org](https://comfy.inco.org/) 
	- Has a "sheilding" mechanism - shows a shielded and unsheilded balance in the web UI
	- Web UI automatically shows `???` for your shielded balance, but apparently can see it readily if you click the "reveal" eye icon
	- Right now its on base sepiola
	- Get testnet tokens for Base sepiola: https://faucet.circle.com/
		- This doesn't actually work for some reason #TODO update link with better link
- https://github.com/Inco-fhevm
	- [confidential ERC20 framework repo](https://github.com/Inco-fhevm/confidential-erc20-framework/) - and my [[Inco Confidential ERC20]]
		- Has "compliant" and regular confidential ERC20s
			- [ ] Whats the difference #TODO 
		- There are special types - looks like "encrypted" versions of the regular types
			- For example, [there's `ebool`](https://github.com/Inco-fhevm/confidential-erc20-framework/blob/bb39e4f788742121f2fc93de33af58758360545b/contracts/CompliantConfidentialERC20/CompliantConfidentialERC20.sol#L36-L37) - this is just an encrypted bool, stored in type `uint256` - [declaration of `ebool`](https://web.archive.org/web/20250108173829/https://github.com/zama-ai/fhevm/blob/main/lib/TFHE.sol)
		- [ ] Went looking for TFHE.sol, and the main result on search engines is a github repo. That appears to be down. But, you can find it on the wayback machine! #TODO figure out why this is down. Maybe it was renamed
			- [site which is down](https://github.com/zama-ai/fhevm/blob/main/lib/TFHE.sol) 
			- [same site on the wayback machine hehehehe](https://web.archive.org/web/20250108173829/https://github.com/zama-ai/fhevm/blob/main/lib/TFHE.sol)
		- Looks like Inco and Zama are working closely together. The `TFHE` i see in their contracts i think comes from Zama, but haven't confirmed it in the code as a dependency. 
			- [Zama FHEVM library github](https://github.com/zama-ai/fhevm-solidity)
- [Inco example contracts repo](https://github.com/Inco-fhevm/inco-example-contracts/tree/main/contracts)
	- Has lots of contracts that seem promising, though not sure how developed they are
		- Private voting
		- Blind auction
		- DID
		- "Pokemon card packs" 
			- Small [typo on L42](https://github.com/Inco-fhevm/inco-example-contracts/blob/a9dcf8b06fd750e0a83e9143d84ccc94fc9a7d86/contracts/PokemonCardPacks.sol#L42) - `Encrypte Key` should say something more like `Encrypted Key` #TODO Report? 
- [[Inco Confidential Tokens]]

### GDPR requirements
_GDPR is "General Data Protection Regulation" and applies to European companies. They have a duty to follow these regulations when it comes to E.U. Citizens_
- Any information that can "identify a natural person" is considered personal data and is protected
	- Wallet addresses
	- TX metadata (Transaction IDs, *timestamps*)
		- #TODO timestamps! Hmmmm how much of a requirement is this? This is interesting and may mean we need time-delaying mixers or something
	- KYC/AML data (names, addresses or other ID info)
- GDPR has a specified "right to be forgotten" which conflicts with immutable information