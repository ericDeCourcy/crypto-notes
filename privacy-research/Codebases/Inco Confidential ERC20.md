### ConfidentialERC20.sol
- [`totalSupply` is not encrypted](https://github.com/Inco-fhevm/confidential-erc20-framework/blob/bb39e4f788742121f2fc93de33af58758360545b/contracts/ConfidentialERC20/ConfidentialERC20.sol#L90-L92) 
	- [ ] If the token is mintable, this leaks some privacy whenever said token is minted
		- [ ] Indeed, the [mint function](https://github.com/Inco-fhevm/confidential-erc20-framework/blob/bb39e4f788742121f2fc93de33af58758360545b/contracts/ConfidentialERC20/ConfidentialERC20.sol#L225-L233) directly increases the total supply, while also revealing exactly which address it has minted to (addresses are visible in plaintext)
			- [ ] Is there some kind of `eAddress` ? Like an encrypted version of the address?
- addresses on `_transfer` calls are publicly visible
	- [`_transfer` function](https://github.com/Inco-fhevm/confidential-erc20-framework/blob/bb39e4f788742121f2fc93de33af58758360545b/contracts/ConfidentialERC20/ConfidentialERC20.sol#L191-L207) in question
- [ ] Why do they [apparently "allow" both `address(this)` and the "to" address](https://github.com/Inco-fhevm/confidential-erc20-framework/blob/bb39e4f788742121f2fc93de33af58758360545b/contracts/ConfidentialERC20/ConfidentialERC20.sol#L201-L202) within the `_transfer` function?