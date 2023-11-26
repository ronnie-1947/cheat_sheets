# Solidity

## Common patterns to remember

### Get my address like this 😎

```
address(this).balance;
address(this).sender;
```

### Transfer eth

```
address public _to;
_to = msg.sender;
_to.transfer(address(this).balance); // Transfer eth
```

## Basic structure

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.14;

contract stringExample {
    string public myString = "hello world"; // public is used to view message
}
```

## Data Types

### Booleans

Denoted by the type "bool". Here is an example 👇

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.14;

contract ExampleBoolean {
    bool public myBool;

    function setBool(bool _myBool) public {
        myBool = _myBool;
    }
}
```

### Unsigned & Signed Integers

Unsigned integers are **Positive integers** from 0 to 2^8. It is denoted by uint. uint is alias for uint256. Value can range from 0-2^256
Signed integers can be both **Positive and Negative**. The type is written by "int"

Here is an example 👇

```
pragma solidity 0.8.14;

contract ExampleInt {
  uint public myUint; // default is 0
  int public mySignedInt = -10;

  function setMyUint(uint _myUint) public {
    myUint = _myUint;
  }
}
```

### Strings & Bytes

Strings are expensive to store in a blockchain. Bytes are string like.

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.14;

contract stringExample {
  string public myString = "hello world";
  bytes public myBytes = "hello as bytes"; // Just a byte view. bytes.length is possible

  function setMyString (string memory _myString) public {
    myString = _myString;
  }

  function compareString(string memory _myString) public view returns(bool){
    return keccak256(abi.encodePacked(myString)) == keccak256(abi.encodePacked(_myString)); // Compare hashes of the strings
  }
}
```

### Address type

This is an intersting one. We set address type

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract addressExample {

    address public myAddress;

    function addAddress(address _myAddress) public {
        myAddress = _myAddress;
    }

    function getAddressBal() public view returns (uint){
        return myAddress.balance;
    }
}
```

### Global object "msg"

msg is a global object. it has some properties such as address, balance etc. To use it use the snippet below

```
function addSenderAddress() public {
        myAddress = msg.sender;
    }
```

### Constructor

Just like JS class, constructor can be written. Constructor runs during **deploying a contract**.

```
pragma solidity 0.8.15;

contract addressExample {

    address public myAddress;

    constructor(address _randomAddress){
        myAddress = _randomAddress;
    }
}
```

## Reading Function types (view || pure)

**Pure:** Pure functions give same inputs on same args. Like pure functions in JS. They can't read from public vars.

**View:** These are impure functions and can give results from other environments.

```
contract addressExample {

    uint public myStorageVar;

    // Reading functions
    function getMyStorageVar() public view returns (uint){
        return myStorageVar;
    }

    function pureFunction(uint a, uint b) public pure returns (uint){
        return  a+b;
    }
}
```

## Mapings and Structs (Types of hashmaps)

### Mapings (Objects and Arrays in solidity)

Mapping are objects in solidity. Below a snippet is given, how to use it.👇

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract ExampleMapping{

    mapping (uint=>bool) public myMapping;
    mapping (uint=>address) public myAddressMapping;
    uint idx;


    function setValue(uint _index) public {
        myMapping[_index] = true;
    }

    function setMyAddress() public {
        myAddressMapping[idx] = msg.sender;
        idx+=1;
    }
}
```

### Structs

Structs are type of objects with any type of key and value

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.15;

contract Wallet {

    struct PaymentReceivedStruct{
        address from;
        uint amount;
    }

    PaymentReceivedStruct public payment;

    function payContract() public payable {
        //payment = PaymentReceivedStruct(msg.sender, msg.value); // One way of writing into payments

        payment.from = msg.sender;  // Another way
        payment.amount = msg.value;
    }
}
```

## Error Handling

### Require keyword

Require keyword is amazing. It rolls the entire transaction if require value is not fulfilled. Require keyword is used for input validation.It returns remaining gas. Here is an example 👇

```
contract Wallet{
  mappint(address=>uint) public balanceReceived;

  function receiveMoney() public payable {
    balanceReceived[msg.sender] += msg.value;
  }

  function withdrawMoney(address payable _to, uint _amount) public {
    require (_amount <= balanceReceived[msg.sender], "Not enough funds, aborting!!!");  // 👈
    balanceReceived[msg.sender] -= _amount;
    _to.transfer(_amount);
  }
}
```

### Try Catch

## Events

Events are a way to access this logging facility.

```
//SPDX-License-Identifier: MIT
pragma solidity 0.8.16;

contract EventExample {

  mapping(address => uint) public tokenBalance;

  event TokensSent(address _from, address _to, uint _amount);

  constructor() {
    tokenBalance[msg.sender] = 100;
  }

  function sendToken(address _to, uint _amount) public returns(bool) {
    require(tokenBalance[msg.sender] >= _amount, "Not enough tokens");
    tokenBalance[msg.sender] -= _amount;
    tokenBalance[_to] += _amount;

    emit TokensSent(msg.sender, _to, _amount);

    return true;
  }
}
```

## Modifiers

These are type of middlewares or functions that calls a callback function

```
contract InheritanceModifierExample {

  mapping(address => uint) public tokenBalance;

  address owner;
  uint tokenPrice = 1 ether;

  constructor() {
    owner = msg.sender;
    tokenBalance[owner] = 100;
  }

  /*==== Modifier Begins ====*/ 
  modifier onlyOwner() {
    require(msg.sender == owner, "You are not allowed");
    _;
  }

  function createNewToken() public onlyOwner {
    tokenBalance[owner]++;
  }
}
```
