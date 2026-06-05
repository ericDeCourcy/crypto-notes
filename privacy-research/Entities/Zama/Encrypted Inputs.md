[Encrypted Inputs Page in Zama Docs](https://docs.zama.ai/protocol/solidity-guides/v0.7/smart-contract/inputs#how-validation-works)

What's going on here? Looks like these involve a zkProof. Basically, an ecrypted type of input also comes with a dynamic-length `proof`, which validates that the msg.sender knows the value of the encrypted input.

The "encrypted types" are `externalEbool`, `externalEaddress`, `externalEuintXX`. They represent the index of the encrypted data within the proof.

#TODO what about the types `eaddress`, `ebool` and `euintXXX`? What is the difference between these and the "external" versions?