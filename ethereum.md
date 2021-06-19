Metamask is a wallet that allows your web browser to interact with the ethereum
blockchain network.

It can connect to both mainnet (the live blockchain) as well as various testnets
such as Rinkeby, Ropsten, Kovan etc.

Web3.js as well as Web3.py are libraries that interact with the Ethereum
blockchain, they allow the client to read and write to the Ethereum blockchain.

### Using Web3.js

```bash
mkdir web3project
cd web3project

npm init
npm install web3 --save

-- open node in the terminal
node

var Web3 = require('web3')
```

### Components of the Web3.js library

* Web3 is the main class of anything related to Ethereum
* web3.eth allows you to interact with the blockchain and smart contracts
* web3.eth.accounts contain functions to generate Ethereum accounts and sign
tractions and data
* web3.eth.personal can interact with the Ethereum nodes accounts
* web3.utils provides utility functions for Ethereum dapps 

### Using Infura to connect to Ethereum

We an use Infura as a provider that allows us to connect to the Ethereum
blockchain, without having to run a full note. 

The service can be signed up for at http://infura.io.

It has access to Mainnet, Ropsten, Kovan and Rinkeby

### Connect to the Etherum network

```javascript
let Web3 = require('web3')
let URL = '<your infura endpoint url>'

let web3 = new Web3(URL)
let address = '0xA1b591b1248a1F30C9d5Fb3811E8b416FeA1'
web3.eth.getBalance(address, (err,bal) => { balance = bal }
```
### Ethereum Accounts (Identity)

There are two types of accounts Externally Owned Accounts (EOA's) and Contract
Accounts (CA's). The EOA's are owned by users with a private key whilst CA's are
held by smart contracts.

#### Externally Owned Account (EOA)

Contents: 
 - Account Balance
 - Transaction Count

Abilities: 
 - Send transactions
 - Initiate smart contracts
 - Transfer value from its wallet

#### Contract Accounts (CA)

Are controlled by the code in the smart contract

Contents: 
 - Account Balance
 - Transaction Count (number of times it has deployed other CA's)
 - Smart Contract Code

Abilities: 
 - Transfer value
 - Initiate other smart contracts
 - Execute smart contracts
 - Manipulate storage 

#### Account State Variables are
 - Nonce (the number of transactions in EOA or contracts created CA)
 - Account Balance (balance available in the account in Wei)
 - Storage Hash (the root node of the patricia tree)
 - Code Hash (hash of code in the contract always executed when called)

Note because the code hash cannot change, the contract cannot change once it has
been deployed.

 
 EOA  |  CA
 -----|-----
Tied to private key     | 
Doesnt hold code        | Has code 
Maintains ether balance | Maintains ether balance
Can send transactions   | Executed code when triggerd by transaction 

# :rocket:

### Read an Externally Owned Account

An example of converting a balance from Wei into Ether is 

```javascript
let address = '0xAb5801a7D398351b8bE11C439e05C5B3259aeC9B'
web3.eth.getBalance(address).then(data => balance = data)
web3.utils.fromWei(balance, 'ether')

// an example of getting a transaction count
web3.eth.getTransactionCount(address)
 .then(console.log)

// using async await syntax 

(async () => {
    let balance = await web3.eth.getBalance(address)
    return web3.utils.fromWei(balance, 'ether')
})()

(async () => {
    let count = await web3.eth.getTransactionCount(address)
    console.log(count)
)()
```
### Read a contract account

The process is get the contract address, find the abi, and then use values as
parameters to the function web3.eth.Contract

```javascript
var ABI = <get abi from the contract on etherscan.io>
var contractAddress = '0x0D8775F648430679A709E98d2b0Cb6250d2887EF'
var contract = new web3.eth.Contract(ABI, contractAddress)
contract.methods

// retrieve values 

contract.methods.name().call().then(data => console.log(data))
contract.methods.symbol().call().then(data => console.log(data))
contract.methods.totalSupply().call().then(data => console.log(data))
```

