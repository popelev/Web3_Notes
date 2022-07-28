
### SWC-100 Function Default Visibility
```solidity
function _sendWinnings() {  } // public / private ... ?
```
  
### SWC-101 Integer Overflow and Underflow
```solidity
uint16 count = 0; 
function run(uint16 input) public { 
	count -= input; // 0 - 1 = 65535
}
```
  
### SWC-102 Outdated Compiler Version
```solidity
pragma solidity 0.4.13; // old version
```
  
### SWC-103 Floating Pragma
```solidity
pragma solidity ^0.4.0; // should be fixed version which was in unit tests
```
  
### SWC-104 Unchecked Call Return Value
```solidity
function callnotchecked(address callee) public { 
	callee.call(); // add "(bool success, ) = ..."
}
```
  
### SWC-105 Unprotected Ether Withdrawal
```solidity
function withdrawAll() { // onlyOwner or WhiteList
	msg.sender.transfer(this.balance);
}
```
  
### SWC-106 Unprotected SELFDESTRUCT Instruction
```solidity
function sudicideAnyone() { // onlyOwner or WhiteList
	selfdestruct(msg.sender); 
}
```
  
### SWC-107 Reentrancy
```solidity
// import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
// and use nonReentrant() modifier
```
  
### SWC-108 State Variable Default Visibility
```solidity
contract Storage {
	uint storeduint1 = 15; // public / private ... ?
}
```
  
### SWC-109 Uninitialized Storage Pointer
```solidity
struct Game { address player; uint256 number; }
function play() payable public { 
	Game game; // add "storage" or "memory"
	...
}
```
  
### SWC-110 Assert Violation
```solidity
function AssertConstructor() public { 
	assert(foo() == 10); //always assert violation
}
function foo() returns(uint){ 
	return 11; 
}
```
  

### SWC-111 Use of Deprecated Solidity Functions
```solidity
suicide(address) => selfdestruct(address)  
block.blockhash(uint) => blockhash(uint)  
sha3(...) => keccak256(...)  
callcode(...) => delegatecall(...)  
throw => revert()  
msg.gas => gasleft  
constant => view  
var => "corresponding type name"  
```
  
### SWC-112 Delegatecall to Untrusted Callee
```solidity
function forward(address callee, bytes _data) public {
	(bool success,) = callee.delegatecall(_data);
	// "callee" should be set in constructor 	
}
```
  
### SWC-113 DoS with Failed Call
```solidity
address[] private refundAddresses; 
mapping (address => uint) public refunds;

function refundAll() public { 
	for(uint x; x < refundAddresses.length; x++) { 
		// arbitrary length iteration based on how many addresses participated 
		require(refundAddresses[x].send(refunds[refundAddresses[x]])); 
		// doubly bad, now a single failure on send will hold up all funds 
	} 
}
// use Pull over Push 
```
  
### SWC-114 Transaction Order Dependence
[Frontrunning](https://consensys.github.io/smart-contract-best-practices/attacks/frontrunning/)  
  
[example: ERC20 approve() front running](https://www.adrianhetman.com/unboxing-erc20-approve-issues/)
  
### SWC-115 Authorization through tx.origin
```solidity
function sendTo(address receiver, uint amount) public { 
	require(tx.origin == owner); // do not use tx.origin to authorize 
	receiver.transfer(amount); 
}
```
  
### SWC-116 Block values as a proxy for time
*Developers should write smart contracts with the notion that block values are not precise, and the use of them can lead to unexpected effects. Alternatively, they may make use oracles.
```solidity
function isSaleFinished() private returns (bool) { 
	return block.timestamp >= 1546300800; 
	// use block.timestamp and block.number and with warning
}
```
  
### SWC-117 Signature Malleability
[explanation](https://swcregistry.io/docs/SWC-117)
```solidity
function transfer( 
		bytes _signature, 
		address _to, 
		uint256 _value, 
		uint256 _gasPrice, 
		uint256 _nonce) 
	public 
	returns (bool) { 
	bytes32 txid = keccak256(abi.encodePacked(getTransferHash(_to, _value, _gasPrice, _nonce), _signature)); // somesing wrong with "_signature"
	require(!signatureUsed[txid]);
}
```
  
### SWC-118 Incorrect Constructor Name
```solidity
contract Missing{
	function Missing() public { } // constructor in old version. register!
}

constructor() {} // in new versions
```
  
### SWC-119 Shadowing State Variables
```solidity
contract Tokensale { 
	uint hardcap = 10000 ether;
}
contract Presale is Tokensale { 
	uint hardcap = 1000 ether; // Shadowing a state variable of Tokensale
//If both is needed - rename one of this
}
```
  
### SWC-120 Weak Sources of Randomness from Chain Attributes
```solidity
randomValue = uint8(keccak256(block.blockhash(block.number - 1), now));
```
  
### SWC-121 Missing Protection against Signature Replay Attacks
[description](https://swcregistry.io/docs/SWC-121)
  
### SWC-122 Lack of Proper Signature Verification
[description](https://swcregistry.io/docs/SWC-122)
  
### SWC-123 Requirement Violation
```solidity
function AssertConstructor() public { 
	foo(0); //always require in foo
}
function foo(int256 x){ 
	require(0 < x); 
}
```
  
### SWC-124 Write to Arbitrary Storage Location
[description](https://swcregistry.io/docs/SWC-124)  
  
### SWC-125 Incorrect Inheritance Order
[description](https://swcregistry.io/docs/SWC-125)  
  
### SWC-126 Insufficient Gas Griefing
[description](https://swcregistry.io/docs/SWC-126)  
    
### SWC-127 Arbitrary Jump with Function Type Variable
[description](https://swcregistry.io/docs/SWC-127)  
    
### SWC-128 DoS With Block Gas Limit
[description](https://swcregistry.io/docs/SWC-128)  
    
### SWC-129 Typographical Error
```solidity
uint number = 1; 
function addOneToNumber() public { 
	number =+ 1; // Always One, should be +=
}
```
  
### SWC-130 Right-To-Left-Override control character (U+202E)
[description](https://skylightcyber.com/2019/05/12/ethereum-smart-contracts-exploitation-using-right-to-left-override-character/)
  
### SWC-131 Presence of unused variables
```solidity
contract DerivedA is Base { 
	A i = A(1); // i is not used in the current contract 
	
	int internal j = 500; 
	
	function call(int a) public { 
		assign1(a); 
	}
}
```
  
### SWC-132 Unexpected Ether balance
[description](https://swcregistry.io/docs/SWC-132)
  
### SWC-133 Hash Collisions With Multiple Variable Length Arguments
[description](https://swcregistry.io/docs/SWC-133)
  
### SWC-134 Message call with hardcoded gas amount
[description](https://swcregistry.io/docs/SWC-134)
  
### SWC-135 Code With No Effects
```solidity
function foo(uint amount) public payable { 
	balance[msg.sender] == amount;  // no effect. may be "=" not "=="?
}
```
  
### SWC-136 Unencrypted Private Data On-Chain
[description](https://swcregistry.io/docs/SWC-136)
  