### Overflow
Arithmetic in the encrypted domain is unchecked to ensure consistent behavior and to ensure that no information is leaked about amounts. So, we must check that the appropriate constraints are used.

- #OpenQuestion If such constraints are used, how do we do so in a way that doesn't leak information that Zama/FHE devs are concerned about?
	- Answer: ensure that the overflow detection and both branches are handled entirely in the encrypted space. If a computation result might overflow, ensure that there is a reasonable "backup" answer in the encrypted case.

### Missing Authorization or Handle-Re-use
#NFP_7

General notion is that someone can take a handle which was already used by someone else, and run it through a contract that has permission to glean information about the output. For example, if a token has permission to modify a handle, calling into the token such that you use a handle that shouldn't belong to you allows you to glean info about said token. 

### Missing or out-of-order invalidations for decryption request IDs
Within Zama/FHEVM contracts, a common pattern is to have a decryption request which produces a "decryptionID", which is then consumed by an asyncronous function which consumes this decryptionID. It should be checked that
1. The decryptionID is only usable once, and is invalidated afterwards
2. The invalidation prevents use of a non-existent ID. For example, it is not possible to transfer to the `address(0)` because that is the default value of "destination" after a request has been deleted
3. The decryption struct (`cleartext`) contains enough information to sufficiently distinguish it from other decryption requests or to clearly fully define behavior for said async action. For example, an async withdrawal function should define the allowed callers and amount.

### Confidential ERC20 Attack Example
See Zama docs for [Confidential ERC20 Attack](https://docs.zama.ai/protocol/solidity-guides/smart-contract/acl/acl_examples#example-scenario-confidential-erc20-attack)

