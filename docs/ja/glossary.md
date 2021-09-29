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