-   `storage` - variable is a state variable (store on blockchain)
-   `memory` - variable is in memory and it exists while a function is being called
-   `calldata` - special data location that contains function arguments


---
**STORAGE**

https://programtheblockchain.com/posts/2018/03/09/understanding-ethereum-smart-contract-storage/

```solidity
contract StorageTest {
    uint256 a;     // slot 0
    uint256[2] b;  // slots 1-2

    struct Entry {
        uint256 id;
        uint256 value;
    }
    Entry c;       // slots 3-4
    Entry[] d;     // slot 5 for length, keccak256(5)+ for data

    mapping(uint256 => uint256) e;    // slot 6, data at h(k . 6)
    mapping(uint256 => uint256) f;    // slot 7, data at h(k . 7)

    mapping(uint256 => uint256[]) g;  // slot 8
    mapping(uint256 => uint256)[] h;  // slot 9
}
```

```javascript
await web3.eth.getStorageAt("contract address", slot_number)
```

```javascript
await ethers.provider.getStorageAt("contract address", slot_number)
```
