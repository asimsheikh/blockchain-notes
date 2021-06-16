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

### Connect to the Etherum network

```javascript
let Web3 = require('web3')
let URL = '<your infura endpoint url>'

let web3 = new Web3(URL)
let address = '0xA1b591b1248a1F30C9d5Fb3811E8b416FeA1'
web3.eth.getBalance(address, (err,bal) => { balance = bal }
```

