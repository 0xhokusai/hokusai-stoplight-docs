## Hokusai APIを始める 🌊

> このページでは Hokusai API を利用するためのチュートリアルを提供します。
[このリポジトリ](https://github.com/0xhokusai/hokusai-get-started)をローカルに clone して、以下のチュートリアルへお進みください。

## チュートリアル
チュートリアルを始めるために、下記よりリポジトリをクローンしてください。
　
```:bash
git clone https://github.com/0xhokusai/hokusai-get-started.git
```

インストールには、次の手順が必要です。
- Obtain API key
- Create your wallet
- Publish NFT metadata
- Access NFT metadata

## 1. API Key の取得

[こちら](https://0xhokusai.notion.site/Hokusai-API-Application-form-a6d8118d416b41d88632396e3156cddb)のフォームからAPI Keyの発行申請を行ってください。
API Key発行まで2~3営業日お待ち下さい。

## 2. ウォレットの作成

NFT を Mint するために、ウォレットアドレスを保持する必要があります。
ウォレットソフトウェアとして、[Metamask](https://docs.metamask.io) を利用することをお勧めします。

Metamask は、Ethereum などのブロックチェーンとやりとりを行う、仮想通貨のウォレットです。
Metamask を利用したことがない方は、以下の記事を参考に Metamask の導入、 Polygon Mumbai Network の設定を行ってください。
- [How to create Metamask Wallet](https://docs.polygon.technology/docs/home/blockchain-basics/import-account-to-metamask)
- [Configure Polygon on Metamask](https://docs.polygon.technology/docs/home/blockchain-basics/import-account-to-metamask)

## 3. NFT のメタデータを公開

NFT は[特別なメタデータ](https://nftschool.dev/reference/metadata-schemas/#intro-to-json-schemas)の URI を保持することができます。
ここでは、メタデータの URI を簡単に公開する方法をご紹介します。

今回は、NFT のメタデータを公開するために、[nft.storage](https://nft.storage/) と呼ばれるサービスを利用します。
[nft.storage](https://nft.storage/) では、[IPFS](https://docs.ipfs.io/) を利用してメタデータの保存・公開をします。
[IPFS](https://docs.ipfs.io/) とは、ファイルやウェブサイト、アプリケーション、データを保存・アクセスするための分散システムです。

以下のコードを実行して、メタデータを公開します。

```bash
cp .env.sample .env # and rewrite API Keys
yarn # install the required packages
yarn store-metadata # and you will publish metadata with hokusai.png
```

以下のような URL が得られます。

```bash
https://dweb.link/ipfs/bafyreieaaqfof34kfqyvwe4arta6jsuwuauim4d24qo22ct2xnvjnlnrb4//metadata.json
```

IPFS上にアップロードされたメタデータに対して、以下のURLからアクセスすることができます。

```bash
curl https://dweb.link/ipfs/bafyreieaaqfof34kfqyvwe4arta6jsuwuauim4d24qo22ct2xnvjnlnrb4/metadata.json

{
    "name":"nft.storage store test",
    "description":"Using the nft.storage metadata API to create ERC-1155 compatible metadata.",
    "image":"ipfs://bafybeicsu73gednfaa5svozuoac4ebpi76nn4auhygcvkvbn4kk2vdv5ey/hokusai.png"
}
```

## 4. Hokusai API を動かす

Hokusai API を動かす準備が整いました。

### NFT を発行する

NFT を Mint してみましょう。
以下のコードを実行してください。

{to}には発行したNFTを受け取るウォレットアドレスを指定します。このチュートリアルでは自分のウォレットアドレスを指定しましょう。

```bash
yarn mint-nft {to} {tokenUri}
```

パラメータの詳細については、[Hokusai API ドキュメント](../../swagger.yaml#mint-a-new-nft) を確認してください。

### NFT を取得する

Mint 時に発行された `tokenId` を利用して、NFT の情報を取得してみましょう。
`txHash` を [polygonscan](https://mumbai.polygonscan.com/) から検索することで、mint時に発行された`tokenId` を調べることができます。

```bash
yarn get-nft {tokenId}
```

### NFTを送信する  
NFTを送信するには下記の２つの手順を踏む必要があります。

1. `.env`に送信元のウォレットのprivate keyを入力します。 private keyの取得方法は[こちら](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key)をご覧ください。

2. 下記コードを実行します。{to}にはNFTの送信先のウォレットアドレスを指定します。　　


```:bash
yarn transfer-nft {to} {tokenId}
{
  txHash: '0xdec77ee7148dc796dd08d656a256e1466daf2763c08cfe104f76e8baf318f3ed' # example Transaction Hash
}
```
> Metamask等ウォレットサービスに対応した場合は送信元ウォレットのprivate keyの管理をする必要がありません。

> private Keyは非常に重要な情報です。他の誰とも共有しないでください。

### NFTを消却する  
NFTの消却を行う際は、0x000...にNFTを送信する必要があります。
詳しくは[こちら](docs/ja/burn.md)のドキュメンとご覧ください。


1. `.env`に送信元のウォレットのprivate keyを入力します。 

2. 下記コードを実行します。


```:bash
yarn transfer-nft 0x0000000000000000000000000000000000000000 {tokenId}
{
  txHash: '0xdec77ee7148dc796dd08d656a256e1466daf2763c08cfe104f76e8baf318f3ed' # example Transaction Hash
}
```

これで、Hokusai API のチュートリアルは終了です。
Hokusai API を利用して、NFT を楽しみましょう！
