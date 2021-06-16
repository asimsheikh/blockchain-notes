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


