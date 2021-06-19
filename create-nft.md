Watched the youtube video https://www.youtube.com/watch?v=p36tXHX1JD8 by
Patrick Collins channel to learn about creating and deploying NFT's.

Use the brownie python package 

Then get the [NFT Mix](https://github.com/brownie-mix/nft-mix).

```bash
pip install eth-brownie
brownie bake nft-mix

cd nft
npm install -g ganache-cli 

cp .env env.bat

# enter the PRIVATE_KEY and WEB3_INFURA_PROJECT_ID with set command
set PRIVATE_KEY=<private_key>
set WEB3_INFURA_PROJECT_ID=<web3 infura project id>

call env.bat
```

In the .env we get the Infura Project ID from infura.io and the private key
from your metamask wallet and on windows set those variables.


