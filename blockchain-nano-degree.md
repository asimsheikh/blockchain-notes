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

