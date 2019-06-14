# introduction
solidity 是一個能夠在 ethereum blockchain 產生 smart contract 的程式語言，跟 javascript 在語法上有點相似，但是是 strong typed language


# starting
可先到 http://remix.ethereum.org 這個 online editor 做練習

## example
```js
pragma solidity ^0.4.17; # 指定現在要寫的 solidity version
# 定義 contract，類似定義一個 class
contract Inbox {
    # declare instance variables，public 代表這個 contract 發佈到 blockchain 上每個人都可使用，使用 public 定義一個 variable 也等於產生一個會回傳自己的 function
    string public message;
    # 與 contract 同名，這是一個 constructor
    function Inbox(string initialMessage) public {
        message = initialMessage;
    }
    # 定義函數，如果是要修改合約的函數就不要 return 任何東西，因為是會扣 ether 的
    function setMessage(string newMessage) public {
        message = newMessage;
    }
    # func name, func type, return types,因前面定義的 string public message;這 func 其實已經沒用了
    # function getMessage() public view returns(string){
    #     return message;
    # }
}
```

## function type
func type | meaning
----------|---------
public    | any one can call this function on network
private   | only this contract can call this function
view      | this func returns data and doesn't modify contract data
constant  | same as view
pure      | func will not modify or even read contract data
payable   | when someone call this func they might send ether along

## external account to create contract trx
field | meaning
------|--------
nonce | how many times the sender has sent a trx
to | -(一般 trx 都要指定給誰，當給空值時代表建立新合約)
data | compiled bytecode of the contract
value | amount of 'Wei' to send to the target address
gasPrice | sender 願意付多少 Wei per gas 來處理這筆 trx
startGas/gasLimit | 這筆 trx 上限是多少 units of gas(例:這筆 trx 有一個加號和乘號總共需要 8 gas，但 gasLimit 只給 6 gas 那這筆 trx 就只會執行到一半，也就是這筆 trx 無法成立!) 
v/r/s | generated from sender's private key and can generate sender address

[gas price ref!](https://docs.google.com/spreadsheets/d/1n6mRqkBz3iWcOlRem_mO09GtSKEKrAsfO7Frgx18pNU/edit#gid=0)

## 'msg' global variable
撰寫 solidity 時，就會存在在全域的變數
property name | property
---------|----------
msg.data|data field
msg.gas|amount of ags
msg.sender|address of sender account
msg.value|amount of ether that was sent with trx 
## basic types
types |
------|
string|
bool  |
int   | integer, positive or negative == int256
int8, int16, ..., int256| ...
uint  | integer,positive == uint256
fixed/ufixed| number with decimal
address| has method for sending money

## reference types
types |
------|
fixed array| unchanging length, single type
dynamic array| single type, can change length
mapping| key value, key has same type,value has same type
struct | collection of key value can have different types

## 示意圖
![solidity flow](./solidity.png)

## file structure
```
package.json
compile.js
deploy.js
contracts
  -Inbox.sol
test
  -test.js
```

## compile
```cmd
yarn add solc
```
```js
'compile.js'
const path = require('path');
const fs = require('fs');
const solc = require('solc');

const inboxPath = path.resolve(__dirname, 'contracts', 'Inbox.sol');
const source = fs.readFileSync(inboxPath, 'utf8');

// bytecode 就是要 deploy 到 blockchain，interface 就是 ABI
module.exports = solc.compile(source, 1).contracts[':Inbox'];
```

## erc20 token example with metamask
[simple tutorial](https://gist.github.com/Ankarrr/561fb3e49bd22847780fb93f0e382f59)
```js
// ----------------------------------------------------------------------------
// 'SecuX' token contract
//
// Deployed to : 0x5A86f0cafD4ef3ba4f0344C138afcC84bd1ED222
// Symbol      : SecuX
// Name        : SecuX Token
// Total supply: 100000000
// Decimals    : 18
//

// ----------------------------------------------------------------------------
// Safe maths
// ----------------------------------------------------------------------------
contract SafeMath {
    function safeAdd(uint a, uint b) public pure returns (uint c) {
        c = a + b;
        require(c >= a);
    }
    function safeSub(uint a, uint b) public pure returns (uint c) {
        require(b <= a);
        c = a - b;
    }
    function safeMul(uint a, uint b) public pure returns (uint c) {
        c = a * b;
        require(a == 0 || c / a == b);
    }
    function safeDiv(uint a, uint b) public pure returns (uint c) {
        require(b > 0);
        c = a / b;
    }
}

// ----------------------------------------------------------------------------
// ERC Token Standard #20 Interface
// https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md

// https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md

// ----------------------------------------------------------------------------
contract ERC20Interface {
    function totalSupply() public constant returns (uint);
    function balanceOf(address tokenOwner) public constant returns (uint balance);
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

// ----------------------------------------------------------------------------
// Contract function to receive approval and execute function in one call
//
// Borrowed from MiniMeToken
// ----------------------------------------------------------------------------
contract ApproveAndCallFallBack {
    function receiveApproval(address from, uint256 tokens, address token, bytes data) public;
}

// ----------------------------------------------------------------------------
// Owned contract
// ----------------------------------------------------------------------------
contract Owned {
    address public owner;
    address public newOwner;

    event OwnershipTransferred(address indexed _from, address indexed _to);

    function Owned() public {
        owner = msg.sender;
    }

    modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }

    function transferOwnership(address _newOwner) public onlyOwner {
        newOwner = _newOwner;
    }
    function acceptOwnership() public {
        require(msg.sender == newOwner);
        OwnershipTransferred(owner, newOwner);
        owner = newOwner;
        newOwner = address(0);
    }
}

// ----------------------------------------------------------------------------
// ERC20 Token, with the addition of symbol, name and decimals and assisted
// token transfers
// ----------------------------------------------------------------------------
contract SecuXToken is ERC20Interface, Owned, SafeMath {
    string public symbol;
    string public  name;
    uint8 public decimals;
    uint public _totalSupply;

    mapping(address => uint) balances;
    mapping(address => mapping(address => uint)) allowed;

    // ------------------------------------------------------------------------
    // Constructor
    // ------------------------------------------------------------------------
    function SecuXToken() public {
        symbol = "SecuX";
        name = "SecuX Token";
        decimals = 18;
        _totalSupply = 100000000000000000000000000;
        balances[0x8a5C03d8E67456d982530645519Be435A650Ee4c] = _totalSupply;
        Transfer(address(0), 0x8a5C03d8E67456d982530645519Be435A650Ee4c, _totalSupply);
    }

    // ------------------------------------------------------------------------
    // Total supply
    // ------------------------------------------------------------------------
    function totalSupply() public constant returns (uint) {
        return _totalSupply  - balances[address(0)];
    }

    // ------------------------------------------------------------------------
    // Get the token balance for account tokenOwner
    // ------------------------------------------------------------------------
    function balanceOf(address tokenOwner) public constant returns (uint balance) {
        return balances[tokenOwner];
    }

    // ------------------------------------------------------------------------
    // Transfer the balance from token owner's account to to account
    // - Owner's account must have sufficient balance to transfer
    // - 0 value transfers are allowed
    // ------------------------------------------------------------------------
    function transfer(address to, uint tokens) public returns (bool success) {
        balances[msg.sender] = safeSub(balances[msg.sender], tokens);
        balances[to] = safeAdd(balances[to], tokens);
        Transfer(msg.sender, to, tokens);
        return true;
    }

    // ------------------------------------------------------------------------
    // Token owner can approve for spender to transferFrom(...) tokens
    // from the token owner's account
    //
    // https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md
    // recommends that there are no checks for the approval double-spend attack
    // as this should be implemented in user interfaces 
    // ------------------------------------------------------------------------
    function approve(address spender, uint tokens) public returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        Approval(msg.sender, spender, tokens);
        return true;
    }

    // ------------------------------------------------------------------------
    // Transfer tokens from the from account to the to account
    // 
    // The calling account must already have sufficient tokens approve(...)-d
    // for spending from the from account and
    // - From account must have sufficient balance to transfer
    // - Spender must have sufficient allowance to transfer
    // - 0 value transfers are allowed
    // ------------------------------------------------------------------------
    function transferFrom(address from, address to, uint tokens) public returns (bool success) {
        balances[from] = safeSub(balances[from], tokens);
        allowed[from][msg.sender] = safeSub(allowed[from][msg.sender], tokens);
        balances[to] = safeAdd(balances[to], tokens);
        Transfer(from, to, tokens);
        return true;
    }

    // ------------------------------------------------------------------------
    // Returns the amount of tokens approved by the owner that can be
    // transferred to the spender's account
    // ------------------------------------------------------------------------
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining) {
        return allowed[tokenOwner][spender];
    }

    // ------------------------------------------------------------------------
    // Token owner can approve for spender to transferFrom(...) tokens
    // from the token owner's account. The spender contract function
    // receiveApproval(...) is then executed
    // ------------------------------------------------------------------------
    function approveAndCall(address spender, uint tokens, bytes data) public returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        Approval(msg.sender, spender, tokens);
        ApproveAndCallFallBack(spender).receiveApproval(msg.sender, tokens, this, data);
        return true;
    }

    // ------------------------------------------------------------------------
    // Don't accept ETH
    // ------------------------------------------------------------------------
    function () public payable {
        revert();
    }

    // ------------------------------------------------------------------------
    // Owner can transfer out any accidentally sent ERC20 tokens
    // ------------------------------------------------------------------------
    function transferAnyERC20Token(address tokenAddress, uint tokens) public onlyOwner returns (bool success) {
        return ERC20Interface(tokenAddress).transfer(owner, tokens);
    }
}
```
