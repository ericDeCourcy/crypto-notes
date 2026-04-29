- https://etherscan.io/address/0xdb9b1e94b5b69df7e401ddbede43491141047db3#code

Allows users to delegate actions and pay for them with ERC20s or other tokens through the magic of [[EIP-7702]]

### Open Questions / TODOs
- [ ] How does this work?
- [ ] ABI encoding for params? Need a refresher on this
- [ ] Is this the only contract that does this?

### How the contract works
#### redeemDelegations function
```
function redeemDelegations(
	bytes[] calldata _permissionContexts,
	ModeCode[] calldata _modes,
	bytes[] calldata _executionCallDatas
)
```
- [ ] #TODO what is `ModeCode`
	- This appears to come from [[EIP-7579]]. Here's [where `ModeCode`is mentioned](https://github.com/erc7579/erc7579-implementation/blob/99cbd34479b9d125651316ff2d92f38611ddf665/src/lib/ModeLib.sol#L55) in the reference implementation.
- [ ] #TODO what does this function actually do?
	- See #NFP_13 [[NFP_13]]
	- See #NFP_14 [[NFP_14]]

