# 3 - Ethereum Smart Contracts, Tokens and Dapps

## 1 - Ethereum Fundamentals and Development Tools

### Create a local blockchain

Ganache is a local Ethereum blockchain that gives you 10 accounts with 100
ether each in them.

You can install Ganache using either the qui installer from the website or as a
command line tool using the following command 

```bash
npm install -g ganache-cli
ganache-cli
```

### Connect to your local Ethereum blockchain

Now that you can run a local blockchain, its time to connect and interact with
it.

```bash
mkdir etherproject
cd ethereproject

npm init -y
npm install web3 --save
node
```

Then in the node console you can create a connection the local blockchain and
find all the accounts for it.

```javascript
var Web3 = require('web3')
var web3 = new Web3('http://127.0.0.1:7545') // use URL for your local bc
var accounts = web3.eth.getAccounts().then(acc => acc)
```
__Connect Web3 to Ganache from an IDE__

Simple place the content of the javacript file named index.js and type the
command 

```bash
node index.js
```

### Create Ethereum Transactions

The goal is to 

* discuss the types of transaction and transaction types in Ethereum
* create a transaction programatically that sends ether between 2 accounts

__Transactions Types__

There are two major transaction types, Message Calls and Contact Creation, they
are initiated by an EOA, ie an account with a private key. They move the state
of the ledger.

            TRANSACTION TYPES 
        |                           |
    Message Calls              Contract Creation

    EOA  ->  EOA                  CA creation
    EOA  ->  CA 

A transaction can contain the following fields

```javascript
let txnCount = web3.eth.getTransactionCount(web3.eth.accounts[0])
var rawTxn = {
    nonce: web3.toHex(txnCount),
    gasPrice: webt.toHex(10000000000),
    gasLimit: web3.toHex(14000),
    to: '00x3d80b31a78c30fc628f20b2c89d7ddbf6e53ced',
    value: web3.toHex(0),
    data: 'some hex values'
} 
```

Note the fields

transaction variable | definition 
---------------------|-----------
Nonce | Transaction count from the senders account
Gas Price | Price per unit of gas you are willing to pay for executing code
Gas Limit | Specifies the max number of computational steps the trx is allowed
Value | The amount of Ether you want to send
Data, Init | Information used to record the creation and execution of smart contracts

### Create a Transaction

The steps at a high level are 

- Install a library to create transactions
- Configure our connection to blockchain using Web3

```bash
npm install ethereumjs-tx
```

```javascript
const Web3 = require('web3');
const EthTx = require('ethereumjs-tx').Transaction;

const URL = 'http://localhost:7545';
const web3 = new Web3(URL);

const privateKey =
  '21f242ab40f1b3b7a133967eec74226fd4e8b5b45b2513b2379d219fe873cc3e';

const runTransaction = async () => {
  const accounts = await web3.eth.getAccounts();
  const nonce = await web3.eth.getTransactionCount(accounts[0]);

  const params = {
    nonce: nonce,
    to: accounts[1],
    value: web3.utils.toHex(web3.utils.toWei('0.5', 'ether')),
    gasPrice: web3.utils.toHex(web3.utils.toWei('10', 'gwei')),
    gasLimit: web3.utils.toHex(21_000),
  };

  const txn = new EthTx(params); // need to add { chain: 'rinkeby'} for testnet
  txn.sign(Buffer.from(privateKey, 'hex'));

  const serializedTxn = txn.serialize().toString('hex');
  const result = await
web3.eth.sendSignedTransaction(`0x${serializedTxn}`);
  console.log(result);
};

runTransaction();
```
### Ethereum gas and fees

Transactions in Ethereum take gas, which is paid for in Ether. It functions as
the rationing device that stops peope from flooding the network with
transctions. 

- Gas Price is the amount you are willing to pay for a miner to process the
transaction
- Gas Limit is the maximum amount of gas a sender is willing to pay for a
  tranaction


### Ethereum metrics

Metrics can be attained from etherstats.net or
[etherscan](http://etherscan.io/charts/)

Some terms and defitions 

Term | Definition 
-----|-----------
Best block | highest block number of the longest valid chain
Uncles | orphaned blocks 
LastBlock | the time since the last mined block in seconds 
AverageBlock | average time between blocks mined
Difficulty | also known as the mining difficulty

You can get statistics on the current chain using the following commands

```javascript
web3.eth.getGasPrice().then(console.log)
web3.eth.getUncle(500, 0) // param is the block number
web3.eth.getBlockTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1").then(console.log)
```

### Improved deployment process

In order to get a more production level setup, as opposed to using remix,
deploying the contract, copying the abi and the contract address and then
interacting with our contract through a webapp. We can use the Truffle
framework to automate most of these tasks.

Using [Truffle](https://trufflesuite.com/truffle)

```bash
mkdir etherproject
cd etherproject

npm install truffle -g
truffle init

truffle compile # compile contracts
truffle migrate # migrations
```

## 2 - Smart Contracts with Solidity

### Smart Contracts and Solidity

__What is a smart contract?__

> Its an account controlled by code 

- Its a piece of code that lives on the blockchain
- A self operating computer program that executes when conditions are met
- Examples are sending money, receiving money, transfer of assets
- Core of the Ethereum blockchain

__Smart Contract Properties__

Field | Description
------|------------
Address | Unique account address
Balance | Amount of ether in account
Storage | Data storage for contract

__Smart Contract Lifecycle__

The smart contract lifecycle is 

 Develop -> Deploy -> Invoke -> Self-Destruct

- __Develop__ We write the contract and compile
- __Deploy__ Once deployed you cannot edit the contract, you get the address
  where the contract was deployed
- __Invoke__ Use the address to run functions on it 
- __Destroy__ Whilst this is optional its important as destroying the
  contract once its not required will stop it from accepting funds


## 3 - Ethereum DAPP

We create two applications here with the goal of 

- Practicing with the tools to develop DAPPS
- Implement Smart Contracts to expose Dapp functionalities
- Learning to test contracts
- Identify ERC-721 Non-Fungible Tokens
- Create your ERC-721

### DApps

A Dapp is a decentralised application. The steps are 

- Write a smart contract
- Deploy to the Ethereum network
- Write a front end to interact with the contract

### Tools required to finish  this lesson

```bash
truffle -v  # if not installed then
npm install truffle -g

mkdir starNotaryV1
cd starNotaryV1
truffle unbox webpack 

truffle develop
```
Truffle will then create a local blockchain on localhost:9545

Next steps are to connect MetaMask to the local blockchain
 
 Copy URL of blockchain -> Select Networks on MetaMask -> Select Custom RPC ->
Enter details -> Add Account using private key 
 
Note: the chainId can be found from truffle console with web3.eth.getChainId()

### Testing with Mocha and Chai

__Mocha__ is a testing framework for Javascript
__Chai__ is an assertion library for Javascript

__Mocha Functions__

Function | Description
---------|------------
it | Run a test and make an assertion
describe | group together it functions
beforeEach | general setup code before each it function
afterEach | executes clean up code after each it function

__Chai__

Is an assertion library 

```javascript
 assert.equal(owner, "Asim Sheikh")
 assert.typeOf(foo, "string")
 assert.lengthOf(foo, 3)
 assert.property(foo, "programming")
```

### Write Smart Contracts in Truffle project

Remove all the files in contracts folder except Migrations.sol 

Save the contract StarNotary.sol

```solidity
pragma solidity ^0.4.20;

contract StarNotary {
    string public starName;
    address public starOwner;

    event starClaimed(address owner);

    contructor() public {
        startName = "Awesome Udacity Star";
    }

    function claimStar() public {
        starOwner = msg.sender;
        emit starClaimed(msg.sender);
    }
}
```

Then go to the migrations folder create a new file called 2_deploy_contracts.js

```javascript
const StarNotary = artifacts.require("StarNotary")

module.exports = (deployer) => {
    deployer.deploy(StarNotary)
}
```

Delete everything in the test folder which is left over from the initial unbox
skeleton.

Then in the test folder add the following code named TestStarNotary.js

```javascript
const StarNotary = artifacts.require('StarNotary')

var accounts;
var owner;

contract('StarNotary', accs => {
    accounts = accs
    owner = accounts[0]
})

it('has the correct name', async () => {
    let instance = await StarNotary.deployed()
    let starName = await instance.starName.call()
    assert.equal(starName, "Awesome Udacity Star")
})
```
Note: that function calls like starName do not cost gas and simply return a
value, they are different to transaction calls which do cost gas and change the
state of the world.

We updated the test file to test for setting an owner and changing an owner

```javascript
const StarNotary = artifacts.require('StarNotary')

var accounts;
var owner;

contract('StarNotary', (accs) => {
    accounts = accs
    owner = accounts[0]
})

it('has correct name', async () => {
    let instance = await StarNotary.deployed()
    let starName = await instance.starName.call()
    assert.equal(starName, "Awesome Udacity Star")
})

it('can be claimed', async () => { 
    let instance = await StarNotary.deployed()
    await instance.claimStar({from: owner})
    let result = await instance.starOwner.call()
    assert.equal(result, owner)
})

it('ownership can be changed', async () => {
    let instance = await StarNotary.deployed()
    let firstOwner = accounts[0]
    instance.claimStar({from: firstOwner})
    var result = await instance.starOwner.call()
    assert.equal(result, firstOwner)

    let secondOwner = accounts[0]
    instance.claimStar({from: firstOwner})
    var result = await instance.starOwner.call()
    assert.equal(result, secondOwner)
}) 
```

In order to run the test files we issue the following commands

```bash
truffle develop
compile
migrate --reset
test
``` 

### Tokenize StarNotary contract using ERC721 

The ERC721 contract is for Non Fungible Tokens which are tokens that have a
unique identity. In order to enable these on the Ethereum blockchain, a standard
was defined. 

[OpenZeppelin](https://openzeppelin.com/contracts/) provides implementations of
this and othe standards so that we can utilise secure and community vetted
contracts and use them in our Dapp applications.


