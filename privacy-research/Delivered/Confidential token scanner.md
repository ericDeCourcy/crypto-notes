See [[ERC7984 scanner]]]
# Retro

# Issues
#TODO push these to GH
- [ ] `analyze-address` doesn't display correct amt of tokens for cUSDC hacker - should display something like 12M
	- [ ] Solution - they actually wrap twice, once is much smaller and they do a bunch of transfers. Then the 12M. Solution is to identify wrap amt in output, after `<BLOCK_NUM>:WRAP` like so: `<BLOCK_NUM>:WRAP {123456.78}`.
- [ ] Explain which scripts do labeling and why they exist. Deprecate scripts that aren't needed
- [ ] scan all cTokens doesn't find finalize unwraps