# Network (chain)

Hokusai v2 supports the following networks.
Include the name of the network where you created your contracts in the API path parameter.

| `{network}`         | Network Name                       |
|---------------------|------------------------------------|
| `ethereum-mainnet`  | Ethereum Mainnet                   |
| `ethereum-rinkeby`  | Ethereum Testnet                   |
| `polygon-mainnet`   | Polygon Mainnet                    |
| `polygon-mumbai`    | Polygon Testnet                    |
| `arbitrum-mainnet`  | Arbitrum Mainnet                   |
| `arbitrum-rinkeby`  | Arbitrum Testnet                   |
| `binance-mainnet`   | Binance Mainnet                    |
| `binance-testnet`   | Binance Testnet                    |
| `astar-astar`       | Astar Mainnet (connedted Polkadot) |
| `astar-shiden`      | Astar Mainnet (connected Kusama)   |
| `astar-shibuya`     | Astar Testnet (connected Tokyo)    |
| `avalanche-mainnet` | Avalanche Mainnet                  |
| `avalanche-fuji`    | Avalanche Testnet                  |


## RPC infomations

RPC informations of each network.  
Can be used to connect the wallet to the network.

<!--
type: tab
title: Ethereum
-->

| Name             | RPC URL                      | Chain ID | Currency | Block Explorer URL           |
|------------------|------------------------------|----------|----------|------------------------------|
| Ethereum Mainnet | https://mainnet.infura.io/v3 | 1        | ETH      | https://etherscan.io	       |
| Ethereum Rinkeby | https://rinkeby.infura.io/v3 | 4        | ETH      | https://rinkeby.etherscan.io |

<!--
type: tab
title: Polygon
-->

| Name            | RPC URL                        | Chain ID | Currency | Block Explorer URL             |
|-----------------|--------------------------------|----------|----------|--------------------------------|
| Polygon Mainnet | https://polygon-rpc.com	       | 137      | MATIC    | https://polygonscan.com        |
| Polygon Mumbai  | https://rpc-mumbai.matic.today | 80001    | MATIC    | https://mumbai.polygonscan.com |

<!--
type: tab
title: Arbitrum
-->

| Name             | RPC URL                         | Chain ID | Currency | Block Explorer URL          |
|------------------|---------------------------------|----------|----------|-----------------------------|
| Arbitrum Mainnet | https://arb1.arbitrum.io/rpc	   | 42161    | ETH      | https://arbiscan.io         |
| Arbitrum Rinkeby | https://rinkeby.arbitrum.io/rpc | 421611   | ETH      | https://testnet.arbiscan.io |

<!--
type: tab
title: Binance
-->

| Name            | RPC URL                                        | Chain ID | Currency | Block Explorer URL          |
|-----------------|------------------------------------------------|----------|----------|-----------------------------|
| Binance Mainnet | https://bsc-dataseed.binance.org               | 56       | BNB      | https://bscscan.com         |
| Binance Testnet | https://data-seed-prebsc-1-s1.binance.org:8545 | 97       | BNB      | https://testnet.bscscan.com |

<!--
type: tab
title: Aster
-->

| Name            | RPC URL                                | Chain ID | Currency | Block Explorer URL         |
|-----------------|----------------------------------------|----------|----------|----------------------------|
| Aster           | https://rpc.astar.network:8545         | 592      | ASTR     | https://astar.subscan.io   |
| Aster Shiden    | https://rpc.shiden.astar.network:8545  | 336      | SDN      | https://shiden.subscan.io  |
| Aster Shibuya   | https://rpc.shibuya.astar.network:8545 | 81       | SBY      | https://shibuya.subscan.io |

<!--
type: tab
title: Avalanche
-->

| Name              | RPC URL                                    | Chain ID | Currency | Block Explorer URL          |
|-------------------|--------------------------------------------|----------|----------|-----------------------------|
| Avalanche Mainnet | https://api.avax.network/ext/bc/C/rpc	     | 43114    | AVAX     | https://snowtrace.io        |
| Avalanche Fuji    | https://api.avax-test.network/ext/bc/C/rpc | 43113    | AVAX     | https://testnet.explorer.avax.network |

<!-- type: tab-end -->

Other networks will be supported in the future !
