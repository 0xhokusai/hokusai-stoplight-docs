## Hokusaiを始める 🌊

> このページでは Hokusai を利用するためのチュートリアルを提供します。
[このリポジトリ](https://github.com/0xhokusai/hokusai-get-started)をローカルに clone して、以下のチュートリアルへお進みください。

## チュートリアル
チュートリアルを始めるために、下記よりリポジトリをクローンしてください。

```bash
$ git clone https://github.com/0xhokusai/hokusai-get-started.git
```

インストールには、次の手順が必要です。
- API Keyの取得
- ウォレットの作成
- NFTのメタデータを公開
- NFTを発行する
- NFTを取得する
- NFTを送信する

## 1. API Keyを取得する

[こちら](https://0xhokusai.notion.site/Hokusai-API-Application-form-a6d8118d416b41d88632396e3156cddb)のフォームからAPI Keyの発行申請を行ってください。
Mainnetでご申請の場合、API Key発行まで2~3営業日お待ちください。Testnetの場合は即時発行されます。

## 2. ウォレットを用意する

NFTを管理するためには、トークンを保持するアカウントが必要です。アカウントの作成、及びトークンの転送に必要な署名をするためにウォレットソフトウェアを使用します。ここでは、一般的に利用されているウォレットソフトウェアであるMetamaskを使って、ウォレットとアカウントを作成し、ネットワークに接続する方法を説明します。すでにウォレットを持っている場合は、次のNFTのメタデータを公開するに進んでください。

### 2.1. Metamaskをインストールする

[こちら](https://metamask.io/download.html)からMetamaskをインストールしてください。対応しているブラウザはChrome, Firefox, Brave, Edgeです。

### 2.2. ウォレットを作成する

1. Metamaskをブラウザで開いてください。
2. **開始**ボタンを押してください。
3. 右の**ウォレットの作成**を選択してください。
    
    ![create_a_wallet.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/52FODQ5ud9k)
    
4. データ収集のポリシーとプライバシーポリシーを読み、どちらかを選択してください。
5. 新しいパスワードを入力し、使用条件に同意した後、**作成**を押してください。
6. ビデオを見て、**次へ**を押してください
7. バックアップフレーズを確認後、メモをしてから**次へ**を押してください。（バックアップフレーズは**絶対に**他人と共有せず、安全な状態で保管してください。）
8. 先ほど確認したバックアップフレーズを順に入力後、**確認**を押してください。
9. **すべて完了**を押してください。

ウォレットが作成され、同時にアカウントが作成されました。

### 2.3. ネットワークに接続する

初期の状態ではMetamaskで作成したアカウントはPolygonのネットワーク上に存在していません。そこで、MetamaskにPolygonのネットワークを追加し、アカウントを接続します。

1. 画面右上の**イーサリアム メインネット**と書いてある場所を押してください。
2. メニュー最下部の**カスタム RPC**を押してください。
    
    ![custom_rpc.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/h0BhERcxMwI)
    
3. それぞれのボックスに以下のように情報を入力してください。これらの情報は[ネットワーク（チェイン）](network.md##rpc接続情報)のページに掲載されています。<br>`新規 RPC URL`は、テーブルに記載のRPC URLを一つ選んで入力してください。選択したURLでネットワークの接続に失敗した場合、別のURLを使ってみてください。<br>今回は、`polygon-mumbai`の情報を使用します。

    
    ![new_network_mainnet.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/unc7jEAbkIo)
    
4. **保存**を押してください。

残高の単位が**MATIC**に、画面右上の表示が**Polygon Mainnet** または **Mumbai Testnet**になっていれば、ネットワークへの接続は成功です。

## **3.** nft.storageのAPI Keyを作成する

NFT は[特別なメタデータ](https://nftschool.dev/reference/metadata-schemas/#intro-to-json-schemas)の URI を保持することができます。 このプロジェクトでは、NFT のメタデータを公開するために、[nft.storage](https://nft.storage/) というサービスを利用します。 [nft.storage](https://nft.storage/) では、[IPFS](https://docs.ipfs.io/) を利用してメタデータの保存・公開をしています。 [IPFS](https://docs.ipfs.io/) とは、ファイルやウェブサイト、アプリケーション、データを保存・アクセスするための分散システムです。

nft.storageを利用するために、nft.storageに登録し、API Keyを作成する方法を説明します。

1. [ここ](https://nft.storage/login/)からnft.storageに登録してください。
2. ログインしたら、右上の**API Keys**を押してください。
3. 右上の**New Key**を押してください。
4. API Keyの名前を入力して、**Create**を押してください。

nft.storageのAPI Keyを作成できました。

## 4. プロジェクトの設定をする

### 4.1. 設定ファイルを作成する

このプロジェクトでは、Hokusai API Key, Hokusai Contract ID, [nft.storage](http://nft.storage) API Key, ウォレットの秘密鍵の情報を`[.env](https://www.npmjs.com/package/dotenv)`ファイルから環境変数として読み取ります。以下のコマンドを実行して、プロジェクト内の`.env.sample`ファイルをコピーした`.env`ファイルを作成してください。

```bash
$ cp .env.sample .env
```

`.env`ファイルを編集して、それぞれの変数を設定します。`WALLET_PRIVATE_KEY`はまだ設定する必要はありません。

```bash
#ウォレットの秘密鍵を入力します。詳しくは「6.3. NFTを送信する」を確認してください。
WALLET_PRIVATE_KEY = "your-private-key"
#nft.storage API Keyを入力します。
NFT_STORAGE_API_KEY = "your-nft-storage-api-key"
#ContractをデプロイしたNetworkを入力します（今回は "polygon-mumbai" です)
CONTRACT_NETWORK = "polygon-mumbai"
#Hokusai API Keyを入力します。
HOKUSAI_API_KEY = "your-hokusai-api-key"
#Hokusai Contract Versionを入力します。
HOKUSAI_CONTRACT_VERSION = "your-contract-version"
#Hokusai contract IDを入力します。
HOKUSAI_CONTRACT_ID = "your-contract-id"
#Hokusai Contract Addressを入力します。
HOKUSAI_CONTRACT_ADDRESS = "your-contract-address"
```

### 4.2. 必要なパッケージをインストールする

プログラムの実行に必要なパッケージを、以下のコードを実行してインストールしてください。

```bash
$ yarn
```

## 5. NFTのメタデータを公開する

NFTを発行する前に、nft.storageを利用してサンプルデータをNFTのメタデータとしてアップロードします。以下のコードを実行してください。

```bash
$ yarn store-metadata
```

URLが表示され、メタデータとしてプロジェクト内の`hokusai.png`、及び名前、説明がアップロードされました。IPFS上にアップロードされたメタデータに対して、そのURLからアクセスすることができます。

```bash
$ curl https://dweb.link/ipfs/bafyreieaaqfof34kfqyvwe4arta6jsuwuauim4d24qo22ct2xnvjnlnrb4/metadata.json

{
    "name":"nft.storage store test",
    "description":"Using the nft.storage metadata API to create ERC-1155 compatible metadata.",
    "image":"ipfs://bafybeicsu73gednfaa5svozuoac4ebpi76nn4auhygcvkvbn4kk2vdv5ey/hokusai.png"
}
```

## 6. Hokusai を動かす

Hokusai を動かす準備が整いました。

### 6.1. NFT を発行する

NFT を Mint してみましょう。
以下のコードを実行してください。

{to}には発行したNFTを受け取るウォレットアドレスを指定します。このチュートリアルでは自分のウォレットアドレスを指定しましょう。

{tokenUri}には事前に[NFTメタデータを公開](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDI0-#5-nft%E3%81%AE%E3%83%A1%E3%82%BF%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B)した際に発行されたURLを貼り付けてください。

```bash
$ yarn mint-nft {to} {tokenUri}
```

パラメータの詳細については、[Hokusai ドキュメント](../../reference/swagger-v2.yaml#mints-new-nft) を確認してください。

get-startedにおける全てのコマンドはデフォルトで Hokusai v2 を呼び出します。
もし、Hokusai v1 を使用したい場合は、コマンド末尾に `:v1` を

```bash
# call Hokusai v1 example
$ yarn mint-nft:v1 {to} {tokenUri}
```

#### 6.1.1. PolygonscanでTokenIDを確認する

発行されたNFTがネットワーク上にあるのを確認するためにはPolygonscanを使います。Polygonscanとは、Polygonネットワーク上に記録された全てのトランザクション、ブロック、アカウントの詳細を閲覧することができるウェブサイトです。

Mumbai テストネット：https://mumbai.polygonscan.com

Polygon メインネット：https://polygonscan.com

![polygonscan_top.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/T7773jY2LH8)

mintのレスポンスとして返却された`txHash`をサーチボックスにコピーしエンターを押してください。

![polygonscan_detail.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/9tB4KnKIDrA)

先ほど実行したmintトランザクションに関する詳細な情報の一覧（ブロック、呼び出したコントラクトアドレス、送信されたトークンなど）が表示されます。**Tokens Transferred** > **For ERC-721 TokenID [`tokenId`]** に送信されたNFTに関する情報が記載されています。`[]`の中の数字が`tokenId`を表しています。（上の画像において`tokenId`は`66`）`tokenId`は他のエンドポイントで利用することになります。


### 6.2. NFT を取得する

Mint 時に発行された `tokenId` を利用して、NFT の情報を取得してみましょう。
`txHash` を [polygonscan](https://mumbai.polygonscan.com/) から検索することで、mint時に発行された`tokenId` を調べることができます。

```bash
$ yarn get-nft {tokenId}
```

### 6.3. NFTを送信する

NFTを送信するには送信元アカウントの署名が必要になります。署名には送信元アカウントの秘密鍵を使用します。

#### 6.3.1. 秘密鍵を取得する

ここではMetamaskで秘密鍵を取得する方法を説明します。

1. Metamaskをブラウザで開いてください。
2. 画面右上アカウントアイコンの下にある縦3点のメニューボタンを押してください。
3. **アカウントの詳細**を押してください。
    
    ![account_details.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/6hDwvlke3Mk)
    
4. **秘密鍵のエクスポート**を押してください。
5. Metamaskのパスワードを入力し、確認を押してください。
6. 秘密鍵をコピーして**完了**を押してください。

`.env`の`WALLET_PRIVATE_KEY`に取得した秘密鍵を設定してください。

```bash
#ウォレットの秘密鍵を入力します。詳しくは「6.3. NFTを送信する」を確認してください。
WALLET_PRIVATE_KEY = "your-private-key"
#nft.storage API Keyを入力します。
NFT_STORAGE_API_KEY = "your-nft-storage-api-key"
#ContractをデプロイしたNetworkを入力します(今回は "polygon-mumbai" です)
CONTRACT_NETWORK = "polygon-mumbai"
#Hokusai API Keyを入力します。
HOKUSAI_API_KEY = "your-hokusai-api-key"
#Hokusai Contract Versionを入力します。
HOKUSAI_CONTRACT_VERSION = "your-contract-version"
#Hokusai contract IDを入力します。
HOKUSAI_CONTRACT_ID = "your-contract-id"
#Hokusai Contract Addressを入力します。
HOKUSAI_CONTRACT_ADDRESS = "your-contract-address"
```

> Metamask等ウォレットサービスに対応した場合は送信元ウォレットのprivate keyの管理をする必要がありません。

> private Keyは非常に重要な情報です。他の誰とも共有しないでください。

#### 6.3.2. 送信を実行する

下記のコードを実行してください。`{to}`にはNFT送信先のアカウントアドレスを指定します。

```bash
$ yarn transfer-nft {to} {tokenId}
{
  txHash: '0xdec77ee7148dc796dd08d656a256e1466daf2763c08cfe104f76e8baf318f3ed' # example Transaction Hash
}
```

#### 6.3.3. 送信履歴をPolygonscanで確認する
[Polygonscan](https://mumbai.polygonscan.com/)を使って、実行した送信の履歴を確認することができます。`txHash`をサーチボックスにコピーして検索してください。

![polygonscan_detail_transfer.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/lOIe8DV0J5E)

**Tokens Transferred**から、送信者と送信先のアドレスや送信されたトークンの`tokenId`などの情報を確認することができます。送信者と送信先のウォレットアドレスが実行時に指定したものと合致していることを確認してください。

### 6.4. NFTを消却する  

下記のコードを実行してください。`{tokenId}`には対象のNFTのTokenIdを指定します。
実行するウォレットがその`tokenId`のNFTを所有していることを確認してください。

1. `.env`に送信元のウォレットのprivate keyを入力します。 

2. 下記コードを実行します。


```bash
$ yarn burn-nft {tokenId}
{
  txHash: '0x67eca6ca63d542f4b01fd60d53feda89ce64f42394c27b77fa3fccbb15244d3c' # example Transaction Hash
}
```

これで、Hokusai のチュートリアルは終了です。
Hokusai を利用して、NFT を楽しみましょう！
