*FCMP stands for "Full Chain Membership Proofs".*

FCMP is a fundamental change with how monero transactions work. Monero transactions use ring signatures, whih means each output is spent by one of sixteen members in a ring. FCMP gets rid of this and instead monero transactions prove that they spent a UTXO that exists somewhere on the blockchain, without revealing which one.

FCMP relies on ZK-proofs. #TODO understand how those are used better.

#TODO is this above explanation right? I feel there may be some confusion between *UTXOs* and *signers*. Whats the deal here? 

#TODO where is the github branch or issue or PR which introduces FCMP?

#TODO will FCMP be compatible with "old" transactions? What other changes are implied or forced by FCMP integration?

# Advantages over Ring Signatures
- Ring signature anonymity set of 16 is reasonable enough to trace, especially if there is only "one hop"
- Decoy selection in ring signatures can leak info
	- The decoy selection algorithm may leak by observing decoys #TODO how big of an issue is this? How are decoys selected
		- Note to self: probably not that important as these will soon be deprecated. May be interesting for privacy studies
- [[Ring Signatures]] 
- Could potentially speed up sync times. Don't really understand how just yet #TODO
- Allows for transaction chaining better than ring signatures. This could enable rollups in the future
# Features

## Linkability Tags
Analogous to key images in ring signatures - these are unique and deterministic per UTXO. So double-spends can be easily detected

## Generalized Bulletproofs
Monero already has been using [[Bulletproofs]] for spend amounts

# See also
