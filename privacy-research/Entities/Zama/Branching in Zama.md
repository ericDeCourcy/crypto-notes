### General 
- `if` no work on confidential values. Instead we have `select`, which is kinda like the `? x:y` syntax.
```
ebool isAbove = FHE.lt(highestBid, bid);

highestBid = FHE.select(isAbove, bid, highestBid);
```

### Branching to non-confidential paths
- See the [zama docs regarding this](https://docs.zama.ai/protocol/solidity-guides/smart-contract/logics/conditions#how-to-branch-to-a-non-confidential-path)
- *there may be many cases where the "public" contract logic will depend on the outcome from a encrypted path...To do so, there are only one way to branch from an encrypted path to a non-encrypted path: it requires a public decryption using the oracle. Hence, any contract logic that requires moving from an encrypted input to a non-encrypted path always requires an async contract logic.*
	- [ ] #OpenQuestion isn't this true of any movement from encrypted to decrypted state? Perhaps not, could just require some signature
	- [ ] #OpenQuestion is there a good example of this?
- [ ] #OpenQuestion For branches, is a feasible workaround to do encrypted calculations for both paths, and then to `select` to one of them?