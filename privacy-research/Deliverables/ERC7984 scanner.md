- [ ] #TODO Check if tokens are generally used "privately" or "not-privately" for transfers
- [ ] #TODO Should there be a default path in the "unwrap" function which just "zeroes" your balance rather than storing it as an euint? This would be easier as if you do an unwrap of the handle representing your balance then you're clearly emptying your balance
	- In [`_update` function of ERC7984](https://github.com/OpenZeppelin/openzeppelin-confidential-contracts/blob/136840e97e6ec7331642821a9bd51c61cca1ebf9/contracts/token/ERC7984/ERC7984.sol#L284-L289)
		- there could be branch where 
```
if (from == address(0)) {
	(success, ptr) = FHESafeMath.tryIncrease(_totalSupply, amount);
	FHE.allowThis(ptr);
	_totalSupply = ptr;
} else {
	euint64 fromBalance = _balances[from];
	require(FHE.isInitialized(fromBalance), ERC7984ZeroBalance(from));
	if(fromBalance.unwrap == amount.unwrap)
	{
		ptr = wrap(0);   //this skips an FHE op
	}
	else
	{
		(success, ptr) = FHESafeMath.tryDecrease(fromBalance, amount);
	}
	FHE.allowThis(ptr);
	FHE.allow(ptr, from);
	_balances[from] = ptr;
}
```
- [ ] There may be a standard user flow we can categorize users by

### Get address Script
This script will return all transactions which have a topic in the "confidential transfer" field which matches some address. For example:

- `node scripts/find-address.js 0xfc534a31e8877dc914989267b124a59d6911576d` 
	- This guy did two unwraps, but otherwise pretty standard behavior
		- wrap, bid, recieve multicall, unwrap
- This address also is interesting - similarly simple setup: `0x3a292b57e41d88309201f2df9cf46230c58008e0`
	- #TODO I would really like to understand where the handles he was using came from
- Havent tried this with the script yet, but...
	- https://etherscan.io/tx/0xcaf5041e6846212fe99b11a3540a1e926d3903dfa7a3782b95affe148e5d72ab#eventlog
	- In this tx, you can see the "confidential amount transferred" is the result of a comparison...
		- `6C9003B48BB2080219AF2B18CC8A6B895B7B9FB9A0FF00000000000000010500` is the conf transfer handle
		- it is the `result` of `FHEifThenElse` where the "control" is the result of a `FheGe` on two of the same handle
			- `519CBD8D0DA06573ACA5C170CC2A7517B0F0646A34FF00000000000000010500` 
				- this handle is compared to itself for FheGe, where the result `54DF...` becomes the control for the if/else above
				- Also subbed from itself, where the result is the "true" case for the Ge thing
					- 

### Basic unwrap tracing script
1. identify an address
2. for this address, identify transactions which involved confidential transfers
3. For the "unproved unwrap", identify the handle which is being unwrapped
	1. Find "unwrap" call - look at topic3 in confidential transfer
4. pull all logs for cUSDT contract interactions with this user, find all instances of "unwrap handle"
5. magically identify where that handle is born

### Deliverable ideas
- Show the proportion of "known" versus "unknown" balances - this will be somewhat inflated because so many balances are "definitely zero" due to unwrapping total balance

### Findings
- Simple UX changes could make token history a lot more hidden
	- Not allowing things to go through without a proof
	- Maybe even removing proving altogether
- Unwraps are not equal - the issue with unwraps is not that they reveal how much your clear-space token balance is, its that they *can* reveal your encrypted balance to be 0 
- For tracking, we don't really need to worry about bids, we can just look at if ZAMA was transferred then multiply the amount transferred by the price `0.05` and subtract that from the user's balance
- If you're not *trying to do it right*, you're doing it wrong. This is not good
	- this is referring to the frontend i think
- all balances are constrained by transfer events. 
	- [ ] Does this mean that the privacy set is constrained? by the possibilities of different balances..? 
- "Bits of randomness" is a way of thinking about how "private" a system is.... maybe? IDK if this makes sense
- Backpropogating all the connections will be difficult, i wonder how we can do that
	- once we have all handles listed in db
	- for every address involved in transfers for cToken
		- analyzeAddress and get all handles associated with  that address
		- for each handle
			- find all instances of handles which are "associated" with this handle. Keep going until there are no new handles, store all these handles in a list (group)
				- [ ] what does associated mean precisely? #TODO
					
			- [ ] from every "unwrap" transaction, find what handle it retroactively defines. Throughout the group, replace this handle with its literal value and attempt to solve if all handles present sum to this handle.
				- [ ] To store this we need two columns:
					- AssociatedHandles (JSON)
					- Equation (String)
						- perhaps the table should be "known value", "known value range", "agebraic value", "associated handles(JSON)"
- Privacy decays over time - as more info becomes known, eventually your balance can also become known.
### Questions this tool answers
- what percent of people are doing "full unwraps" vs "unwraps with proof"
	- cUSDT - 642 with proof, 9519 without proof (full unwrap) --> 93.7% are full unwraps

| cToken | Wrap | Transfer | Transfer_w_proof | Unwraps | Unwrap_w_proof | Total | Percent traceable | Final Scanned block |
| ------ | ---- | -------- | ---------------- | ------- | -------------- | ----- | ----------------- | ------------------- |
| cUSDT  |      |          |                  |         |                |       |                   |                     |
| cUSDC  | 150  | 2        | 54               | 26      | 103            |       |                   |                     |
| cBRON  | 16   | 0        | 42               | 0       | 16             |       |                   |                     |
| ctGBP  | 5    | 0        | 4                | 0       | 4              |       |                   |                     |
| cWETH  | 16   | 0        | 3                | 0       | 18             |       |                   |                     |
| cZAMA  | 19   | 0        | 16               | 4       | 24             |       |                   |                     |


- what percent of transfers are "full transfers" vs "partial transfers"
	- cUSDT - 60 w proof,  53 without --> 46.9% are full transfer
- How much legitimate usage of the token is there vs auction usage
- Can I definitively tell you the confidential token balance of X% accounts, and Y% accounts where the balance is nonzero?
- how often does someone have exactly one sender before withdrawing? (this indicates all funds came from one person)
- how often does someone have exactly one recipient from their sends along with a full withdrawal?
- Lets attempt to label each FHEVM handle - how many of them are we able to say with certainty what their values are?

### For "redeem delegations"
See [[NFP_14]]
