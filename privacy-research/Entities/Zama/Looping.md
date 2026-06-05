- In FHE systems, looping based on encrypted values is illegal
- But, what happens if we actually do that?
	- [ ] #OpenQuestion Can we compile and run this code?
```
euint8 maxValue = FHE.asEuint(6); // Could be a value between 0 and 10
euint8 x = FHE.asEuint(0);
// some code
while(FHE.lt(x, maxValue)){
    x = FHE.add(x, 2);
}
```
- Check out the [suggested approach](https://docs.zama.ai/protocol/solidity-guides/smart-contract/logics/loop#suggested-approach) for the same code:
```
euint8 maxValue = FHE.asEuint(6); // Could be a value between 0 and 10
euint8 x;
// some code
for (uint32 i = 0; i < 10; i++) {
    euint8 toAdd = FHE.select(FHE.lt(x, maxValue), 2, 0);
    x = FHE.add(x, toAdd);
}
```
- General idea is to perform enough loops to possibly fulfill conditions, but then "exit early" within the loop based on some condition