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
Data, Init | Information used to record the creation and execution of smart
contracts

### Create a Transaction

The steps at a high level are 

- Install a library to create transactions
- Configure our connection to blockchain using Web3

```bash
npm install ethereumjs-tx
```

```javascript
t Web3 = require('web3');
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

  const txn = new EthTx(params);
  txn.sign(Buffer.from(privateKey, 'hex'));

  const serializedTxn = txn.serialize().toString('hex');
  const result = await
web3.eth.sendSignedTransaction(`0x${serializedTxn}`);
  console.log(result);
};

runTransaction();
```


