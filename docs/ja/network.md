# ネットワーク（チェイン）

Hokusai v2では、以下のネットワークに対応しています。  
APIのパスパラメータに自身が発行したネットワーク名を含めてください。

| `{network}`         | Network Name                       |
|---------------------|------------------------------------|
| `ethereum-mainnet`  | Ethereum Mainnet                   |
| `ethereum-rinkeby`  | Ethereum Testnet (Deprecated)      |
| `ethereum-goerli`   | Ethereum Testnet                   |
| `polygon-mainnet`   | Polygon Mainnet                    |
| `polygon-mumbai`    | Polygon Testnet                    |
| `arbitrum-mainnet`  | Arbitrum Mainnet                   |
| `arbitrum-rinkeby`  | Arbitrum Testnet (Deprecated)      |
| `arbitrum-goerli`   | Arbitrum Testnet                   |
| `binance-mainnet`   | Binance Mainnet                    |
| `binance-testnet`   | Binance Testnet                    |
| `astar-astar`       | Astar Mainnet (connedted Polkadot) |
| `astar-shiden`      | Astar Mainnet (connected Kusama)   |
| `astar-shibuya`     | Astar Testnet (connected Tokyo)    |
| `avalanche-mainnet` | Avalanche Mainnet                  |
| `avalanche-fuji`    | Avalanche Testnet                  |


## RPC接続情報

各ネットワークのRPC接続情報です。  
Metamaskなどのウォレットとネットワークとの接続に使用できます。

<!--
type: tab
title: Ethereum
-->

| ネットワーク名       | RPC URL                      | チェーンID | 通貨記号   | ブロックエクスプローラのURL        |
|------------------|------------------------------|----------|----------|------------------------------|
| Ethereum Mainnet | https://mainnet.infura.io/v3 | 1        | ETH      | https://etherscan.io	       |
| Ethereum Rinkeby | https://rinkeby.infura.io/v3 | 4        | ETH      | https://rinkeby.etherscan.io |
| Ethereum Goerli  | https://goerli.infura.io/v3  | 5        | ETH      | https://goerli.etherscan.io  |

<!--
type: tab
title: Polygon
-->

| ネットワーク名      | RPC URL                        | チェーンID | 通貨記号   | ブロックエクスプローラのURL          |
|-----------------|--------------------------------|----------|----------|--------------------------------|
| Polygon Mainnet | https://polygon-rpc.com	       | 137      | MATIC    | https://polygonscan.com        |
| Polygon Mumbai  | https://rpc-mumbai.matic.today | 80001    | MATIC    | https://mumbai.polygonscan.com |

<!--
type: tab
title: Arbitrum
-->

| ネットワーク名       | RPC URL                                      | チェーンID | 通貨記号   | ブロックエクスプローラのURL       |
|------------------|---------------------------------------------------|----------|----------|-----------------------------|
| Arbitrum Mainnet | https://arb1.arbitrum.io/rpc	                   | 42161    | ETH      | https://arbiscan.io         |
| Arbitrum Rinkeby | https://rinkeby.arbitrum.io/rpc                   | 421611   | ETH      | https://testnet.arbiscan.io |
| Arbitrum Goerli  | https://arb-mainnet.g.alchemy.com/v2/your-api-key | 421611   | AETH     | https://goerli.arbiscan.io  |

<!--
type: tab
title: Binance
-->

| ネットワーク名      | RPC URL                                        | チェーンID | 通貨記号  | ブロックエクスプローラのURL        |
|-----------------|------------------------------------------------|----------|----------|-----------------------------|
| Binance Mainnet | https://bsc-dataseed.binance.org               | 56       | BNB      | https://bscscan.com         |
| Binance Testnet | https://data-seed-prebsc-1-s1.binance.org:8545 | 97       | BNB      | https://testnet.bscscan.com |

<!--
type: tab
title: Aster
-->

| ネットワーク名      | RPC URL                                | チェーンID | 通貨記号  | ブロックエクスプローラのURL       |
|-----------------|----------------------------------------|----------|----------|----------------------------|
| Aster           | https://rpc.astar.network:8545         | 592      | ASTR     | https://astar.subscan.io   |
| Aster Shiden    | https://rpc.shiden.astar.network:8545  | 336      | SDN      | https://shiden.subscan.io  |
| Aster Shibuya   | https://rpc.shibuya.astar.network:8545 | 81       | SBY      | https://shibuya.subscan.io |

<!--
type: tab
title: Avalanche
-->

| ネットワーク名        | RPC URL                                    | チェーンID | 通貨記号  |  ブロックエクスプローラのURL       |
|-------------------|--------------------------------------------|----------|----------|-----------------------------|
| Avalanche Mainnet | https://api.avax.network/ext/bc/C/rpc	     | 43114    | AVAX     | https://snowtrace.io        |
| Avalanche Fuji    | https://api.avax-test.network/ext/bc/C/rpc | 43113    | AVAX     | https://testnet.explorer.avax.network |

<!-- type: tab-end -->


将来的には、これら以外のネットワークにも対応予定です！
