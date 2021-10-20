# 用語集

### ミント

NFTを発行することをMintと表現します。 詳しくは [こちら](https://help.foundation.app/en/articles/4742869-a-complete-guide-to-minting-an-nft).

### ガス代

すべての取引には、ガス料金と呼ばれる取引手数料を支払う必要があります。
通常、ユーザーは消費したガス料金を支払うのに十分なETHを保有する必要があります。（[Polygon](https://polygon.technology/)では、ユーザーは[MATIC](https://polygon.technology/matic-token/)トークンを保有しなければいけません）。

- [ガスとは｜Ethereum Docs](https://ethereum.org/fi/developers/docs/gas/)


### メタトランザクション

メタトランザクションとは、第三者（リレーヤー)が他のユーザーの取引を送信して、ガス料金を自分で支払うことができる取引のことです。
Hokusai　APIでは、メタトランザクションを利用して送信機能を実装しているため、利用者がガス代を支払う必要がありません。
新規参入者にとって、ガス代に十分なETHを保有ことは簡単なことではありません。そのためHokusaiではユーザーにガス代の負担をなくすために、メタトランザクションを利用しています。

- [What is a Meta-transaction | OpenZepplin Docs](https://docs.openzeppelin.com/learn/sending-gasless-transactions#what-is-a-meta-tx)


### メタデータ

メタデータとは、あるデータに付随する付加的な情報のことを指します。NFTの場合、そのトークンが表す名前、説明、画像データなどが一般的ですが、発行者が必要だと考えればどのような情報を付加することもできます。[Opensea](https://opensea.io)のようなNFTマーケットプレイスでは、以下のようなメタデータの規格が定められています。

- [Metadata Standards | Opensea](https://docs.opensea.io/docs/metadata-standards)


### Polygon Mainnet

Polygon Mainnetとは、Ethereumの大きな問題である

https://ethereum.org/en/developers/docs/scaling/#layer-2-scaling

### Mumbai Testnet

### Contract ID

Contract IDとは、Hokusai APIが提供するコントラクトを一意に区別する識別子です。Hokusai APIは、このIDを使ってHokusaiのデータベースから対応するコントラクトアドレス照合し、NFTの発行や送信などの操作を実行します。Contract IDはHokusai API Key発行時にメールで通知されます。Polygon Mainnetでご利用の場合は、API Key1つにつき1つのコントラクトが作成されます。Mumbai Testnetをご利用の場合は、1つのコントラクトを共有します。

### Token ID

Token IDとは、あるNFTコントラクトが発行するトークンを一意に区別するための識別子です。Hokusai APIが提供するコントラクトでは、そのコントラクトを使って発行した何番目のトークンかを表す数字を使用しています。

### API Key

API Keyとは、通常Web APIの利用権を認証するための文字列です。ユーザーはAPIの利用登録をして、API Keyを取得します。Hokusai APIでは、利用登録後、[Contract ID](./glossary.md#contract-id)とともにメールで通知いたします。Polygon Mainnetでご利用の場合は、一つのAPI Keyにつき一つのContractがご使用いただけます。

### ウォレット

ウォレットとは、ユーザーがブロックチェーンとやりとりをするためのインターフェイスです。アカウントの作成（秘密鍵の作成）、トランザクションの発行、残高の確認など、様々な機能を提供します。

### アカウント

アカウントとは、World Stateを構成するオブジェクトです。ユーザーは、通常ウォレットが生成する鍵からアドレスを指定してアカウントを作成します。NFTは、アカウントアドレスに対して発行、送信などが行われます。

- [ETHEREUM ACCOUNTS | Ethereum Docs](https://ethereum.org/en/developers/docs/accounts/)


### TxHash

TxHashとは”transaction hash”のことで、NFTの発行等、コントラクトのコードを実行する時のトランザクションのhash値です。