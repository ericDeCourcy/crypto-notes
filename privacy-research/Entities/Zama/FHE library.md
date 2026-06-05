[Github link to FHE.sol](https://github.com/zama-ai/fhevm/blob/main/library-solidity/lib/FHE.sol)

This library is the central component of the Zama protocol. It implements encrypted values and the associated operations for them.

### Common Functions
- `wrap`/`unwrap` - changes the "type" of a variable to/from some encrypted type (e.g. `ebool`, `euint256`) from/to `bytes32` for the "handle"
	- data in storage stays the same, just calls it a handle or an encrypted type

# Division
- Division between two encrypted values is not available as of FHEVM 0.7.
- However division is supported if the denominator is in the clear space

### Branching
[[Branching in Zama]] is weird in homomorphic encrypted systems. We cannot make it obvious which branch was chosen, so instead of an `if` operation on encrypted values we have the `select` operation.

[Branching Page in the Zama Docs](https://docs.zama.ai/protocol/solidity-guides/smart-contract/logics/conditions) 

#TODO Insert links to other pages in these notes that are related to the FHE library.

### Handles
#TODO What are "Handles"? How do they function? Explain:
- [ ] Reading
- [ ] Writing
- [ ] Access Control for access
	- [ ] Should this section be moved to "Access Control Lists"?
- Basically, every "handle" is a pointer to a stored value in the FHEVM somewhere. Every FHE operation assigns the result to a new handle, and performs the operation on the FHE MPC side of things.

### Proxies and stuff
- Configuration is read from storage, never from immutables. This means that if you do proxy bullshit then it might fail
	- to get this to work, you need the equivalnt of an initializer. Check out [this example in OZ confidential contracts](https://github.com/OpenZeppelin/openzeppelin-confidential-contracts/blob/931d9a532a2121c88e647dd30bcc18d158e059fc/contracts/mocks/finance/VestingWalletConfidentialFactoryMock.sol#L68) 