https://link.springer.com/content/pdf/10.1186/s13677-020-00176-9.pdf
https://ethereum.github.io/yellowpaper/paper.pdf
https://wiki.learnblockchain.cn/OPCODE_Gas.pdf

https://docs.soliditylang.org/en/v0.8.14/units-and-global-variables.html
https://docs.soliditylang.org/en/v0.8.3/control-structures.html#external-function-calls

**GAS CALCULATION RULES **
https://github.com/wolflo/evm-opcodes/blob/main/gas.md#a0-2-access-sets
https://ethereum.org/en/developers/docs/evm/opcodes/

SSTORE - 20000 + NEW SLOT
SSTORE - 100 + OLD SLOT
SSTORE - Base + Base* X

SLOAD - 1st TIME - 2100
SLOAD - 2nd TIME - 100

MSTORE - 3 

MLOAD - 3

```solidity
pragma solidity ^0.8.13;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Strings.sol";

library FunctionGasEstimation{

    function GasCost(string memory name, function () internal returns (string memory) fun) internal returns (string memory)  

    {

    uint u0 = gasleft();

    string memory sm = fun();

    uint u1 = gasleft();

    uint256 diff = u0 - u1;

    return string.concat(name, " GasCost: ",

                    Strings.toString(diff),  

                    " returns(",sm, ")");

    }
}
```