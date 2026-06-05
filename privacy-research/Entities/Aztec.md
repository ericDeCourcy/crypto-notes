### Open Questions 
#TODO 
- WTF is HONK and ultraHonk 
### Links and such
- [Aztec explorer](https://aztecexplorer.xyz/?network=mainnet) 

### Overview video
*https://youtu.be/93pfZMjKMmQ?si=74IdgYjfu2S1025a*

- devs choose what states/funcs are public and private
	-  #TODO How do they inter-operate?
- Public state is account-based
	- stored in pubstate merkle tree
- private state is UTXO based, commitments are stored in onchain merkle tree
	- private state in raw form is stored in a user's device, and then proved via zk execution proof
	- [[#(Private State) Notes]]
		- Notes are just data which can be nullified, like UTXOs
- Nullifiers are stored in a merkle tree which is public state
- Contracts are written in Noir with framework "Aztec.nr"
	- #TODO read on aztec.nr
- Setting up Dev environment is part of the video
	- #TODO setup dev environment
### (Private State) Notes
- "Notes" are a struct with owner and value (some data) 
- Notes can be spent or unspent
- Nullifiers (stored publicly but not linked)
- raw notes are stored locally, note commitments are stored onchain

### Aztec token
- Used for gas, governance and staking on sequencers
- to run a node need 200k $AZTEC