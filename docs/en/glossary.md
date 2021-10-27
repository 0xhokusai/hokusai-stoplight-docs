# Glossary

### Mint

To mint an NFT means to issue an NFT. See more information [here](https://help.foundation.app/en/articles/4742869-a-complete-guide-to-minting-an-nft).

### Gas Fees
All transactions are required to pay a transaction fee, called a gas fee.
Typically, users must have enough ETH to pay for the gas spent (On [Polygon](https://polygon.technology/), users must have [MATIC](https://polygon.technology/matic-token/) tokens).
It is not an easy task for newcomers to accumulate enough ETH for the gas fee. As a result, Hokusai is providing meta-transaction to cover users' gas fees. 
- [What is Gas | Ethereum Docs](https://ethereum.org/fi/developers/docs/gas/)

### Meta Transactions

Meta-transactions mean transactions that a third party (called a relayer) can send another user's transactions and pay themselves for the gas cost.
By using Hokusai API, Users can easily use meta-transaction.

- [What is a Meta-transaction | OpenZepplin Docs](https://docs.openzeppelin.com/learn/sending-gasless-transactions#what-is-a-meta-tx)

### Metadata

Metadata is data that provides additional information about certain data. NFT metadata typically has properties like name, description and image to represent the token itself, but the minter can add any arbitrary data if necessary. Metadata standards are held for NFT market platforms like [Opensea](https://opensea.io).
メタデータとは、あるデータに付随する付加的な情報のことを指します。NFTの場合、そのトークンが表す名前、説明、画像データなどが一般的ですが、発行者が必要だと考えればどのような情報を付加することもできます。[Opensea](https://opensea.io)のようなNFTマーケットプレイスでは、以下のようなメタデータの規格が定められています。

- [Metadata Standards | Opensea](https://docs.opensea.io/docs/metadata-standards)

### Contract ID

Contract IDs are unique strings to identify a contract deployed by Hokusai API. Hokusai API finds from our database the contract address corresponding to the ID, and execute NFT operations such as mint and tranfer. Your contract ID is sent to you via email when your API Key is created. For the Polygon Mainnet plan, one contract (and contract ID) is created for each API Key issued. For the Mumbai Testnet plan, all the users share one contract.

### Token ID

A Token ID is a unique string that identifies an NFT minted with an NFT contract. The token ID of the NFT minted by our contract is the number of tokens the contract has minted.

### API Key

An API Key is a unique identifier used to authenticate a user, developer, or calling program to an API. Usually, users must register for a key to create one. Hokusai API will send you an email with your API Key along with your [Contract ID](./glossary.md#contract-id) after submitting your registration. If you register for a Polygon Mainnet key, a new contract will be deployed for the key.

### Wallet

A wallet is a user interface to a blockchain. A wallet application provides users with a variety of functionalities such as account creation (key pair generation), sending a transaciton to another account, checking your balance, etc. 

### Account

An account is an entity with a unique address that can send transactions. NFTs are minted to account addresses and are held by accounts. Users can create an account with the private key which a wallet application typically generates.

- [ETHEREUM ACCOUNTS | Ethereum Docs](https://ethereum.org/en/developers/docs/accounts/)

### Transaction

A transaction is an object sent to a blockhain network by an externally-owned (a.k.a. human-owned) account. This includes coin transers and contract calls.

- [TRANSACTIONS | Ethereum Docs](https://ethereum.org/en/developers/docs/transactions/)

### TxHash

TxHash is a hash of a transaction for a mint, contract execution, etc.