[Zama Docs HCU page](https://docs.zama.org/protocol/solidity-guides/development-guide/hcu)
- HCU's have a depth and a limit and per-transaction limit of 5M and 20M on devnet (*according to docs as of july 18 2026*)
- If a transaction exceeds either of those limits, it will revert.
- All the costs are listed, here's the ones for `euint32` operations
	- I wanted to copy them in but its a PITA
	- euint32 `add` costs `95k` HCU
	- `not` is the most special, just costing `2` HCU. Casting and trivialEncrypt both cost 32. 
	- most bool operations cost about `20k`
	


