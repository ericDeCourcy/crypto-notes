- They have built a FHE coprocessor that works with EVM-compatible chains
	- Symbolic execution is performed on-chain, while real computation is done off-chain by FHE coprocessors
- When execution occurs on chain, a pointer to off-chain cyphertext results is published on-chain. The FHE coprocessor will then compute the needed data and store it at that pointer
	- [ ] #TODO #OpenQuestion #Bug check to see how these pointers are created - it is likely a hash, so check for collisions. Multiple chains are potentially used, so make sure that there can't be collisions from one chain to the next. 
		- [ ] Ensure something like chainID is included in the hash.
		- Note there may not even be a hash included here
	- #NFP_8

### Coprocessor
[[Zama Coprocessor]]

### HCU
[[Zama HCU]]

### ACL
The ACLs are different contracts that manage decryption privileges

Claude gave me [this address (`0xcA2E8f1F656CD25C01F05d0b243Ab1ecd4a8ffb6`)](https://etherscan.io/address/0xcA2E8f1F656CD25C01F05d0b243Ab1ecd4a8ffb6#code) as the ACL for mainnet ethereum 
### Steakhouse cUSDC Vaults
- Announced [in this tweet](https://x.com/zama/status/2067187076261421067) - alt [link to blogpost](https://www.zama.org/post/morpho-zama-steakhouse-launch-confidential-usdc-vault)
	- Steakhouse Confidential USDC Prime vault is for cUSDC holders to deposit into in Morpho
	- *"Position size, direction, and entry timing stay encrypted."* according to this tweet-post
		- #TODO validate these claims
	- Deposit via the zama app (app.zama.org) 

### Zama governance (DAO)
- Aragon DAO on ethereum
- Related to operations and actions involving the $ZAMA token

## $ZAMA token
- Lives on ethereum 
- is a [[LayerZero]] OFT
	- #TODO what is an OFT?

## Non-EVM chains
- [[HyperLiquid]]
- [[Solana]]

### Links
- [Zama Bounty Program](https://github.com/zama-ai/bounty-program)
- [Zama docs](https://docs.zama.ai/protocol)
- [Zama fhEVM Docs](https://docs.zama.ai/fhevm)
- [Zama litepaper](https://docs.zama.ai/protocol/zama-protocol-litepaper) 
### Audits
- #NFP_9

### Design
- KMS (Key Management System) nodes hold part of the FHE key. We must trust that they are not colluding because if so, they can decrypt the entire private data set.
- There's an L3 Arbitrum... thingy ( #TODO better describe this) that Zama uses for data
	- Relayers submit decryption requests and return data back to EVM chains
	- users can run their own relayers to help with liveness in case their data isn't getting relayed
- #NFP_10
- **GATEWAY:** The central orchestrator of the protocol
	- validates encrypted inputs
	- manages ACLs or Access Control Lists
	- bridges cyphertext across chains
	- coordinates coprocessors and the KMS
- **[[HOST CONTRACTS]]:** 
	- Interact with encrypted data (handles)
    - Perform access control operations - maintaining and enforcing Access Control Lists for ciphertexts
    - Emit events for the off-chain components (coprocessors, Gateway)
- #NFP_11


### Security considerations
See [[Security In Zama]]

### Open Questions
#OpenQuestion 
- [ ] If all encrypted values are in the same FHE system, does knowing the cyphertext and the plaintext allow someone to understand the encryption key? And thus be able to decrypt other values?
- [ ] Surely the MPC engine is not free to run - how does Zama recoup some of the costs of having an entire MPC setup running for encrypted values? is there some way to DDoS them or just grief their system?
	- Aryeh has mentioned that Zama wanted the standard for ERC7984 to be uint64 amounts due to computational concerns, so this idea has some legs
- [ ] What about relabeling certain handles to cause math issues? For example, re-labelling an `euint64` as `euint32`
	- According to chatgpt, that's not allowed:
		- *- FHEVM handles are structured bytes that include a **type byte** (among other fields like chain ID and version). So the system can detect when a handle is used with the wrong type. “Re-labeling” an `euint64` handle as `euint32` would conflict with the embedded type and fail checks. [GitHub](https://github.com/zama-ai/fhevm-backend/issues/429)
	    - *The SDK and on-chain APIs also require the correct **FhevmType** when you decrypt or verify inputs; mismatched types are rejected [docs.zama.ai+1](https://docs.zama.ai/protocol/solidity-guides/getting-started/quick-start-tutorial/test_the_fhevm_contract?utm_source=chatgpt.com) **
	