- #TODO how often is it that the same transaction is present in rings in quick succession? For example, imagine this scenario:
	```
	Alice recieves some monero. She wants to mix it around so that it is harder to trace. She decides that sending it to 3 new addresses (sending it to herself 3 times) ought to work well.
	
	TX 1: 
	Original_UTXO + 15 decoys --> New_UTXO_1
	
	TX 2: 
	New_UTXO_1 + 15 decoys --> New_UTXO_2
	
	TX 3: 
	New_UTXO_2 + 15 decoys --> New_UTXO_3
	```
	- It seems like if these transactions happened in quick succession (within the same hour) PLUS all the decoys came from outside this range, it could be fairly clear what is happening. 
	- Tracing could be complicated if `New_UTXO_X` is ever used in a different transaction as a decoy 