# Send ether
---

**send()** = eth + 2300 gas + fail monitoring **DO NOT USE**

```Solidity
function sendEther(uint _amount) public payable {
	if (!address(receiverAdr).send(_amount)) {
		//handle failed send
	}
}
```


**transfer()** = eth + 2300 gas + without fail monitoring **DO NOT USE**

```Solidity
function transferEther(uint _amount) public payable {
	address(receiverAdr).transfer(_amount);
}
```


**call().value** = eth + no limits of gas + fail monitoring

```Solidity
function callValueEther(uint _amount) public payable {
	require(address(receiverAdr).call.value(_amount).gas(35000)());
}

```

**OLD VERSION**
```solidity
pragma solidity 0.6.0;

(bool success, ) = _addr.call.value(msg.value)("")
```

**NEW VERSION**
```SOLIDITY
pragma solidity ^0.8.13;

(bool sent, bytes memory data) = _to.call{value: msg.value}("");
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

contract ReceiveEther {
    /*
    Which function is called, fallback() or receive()?

           send Ether
               |
         msg.data is empty?
              / \
            yes  no
            /     \
receive() exists?  fallback()
         /   \
        yes   no
        /      \
    receive()   fallback()
    */

    // Function to receive Ether. msg.data must be empty
    receive() external payable {}

    // Fallback function is called when msg.data is not empty
    fallback() external payable {}

    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}

contract SendEther {
    function sendViaTransfer(address payable _to) public payable {
        // This function is no longer recommended for sending Ether.
        _to.transfer(msg.value);
    }

    function sendViaSend(address payable _to) public payable {
        // Send returns a boolean value indicating success or failure.
        // This function is not recommended for sending Ether.
        bool sent = _to.send(msg.value);
        require(sent, "Failed to send Ether");
    }

    function sendViaCall(address payable _to) public payable {
        // Call returns a boolean value indicating success or failure.
        // This is the current recommended method to use.
        (bool sent, bytes memory data) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
    }
}

```