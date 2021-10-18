## Hokusai APIを始める 🌊

> このページでは Hokusai API を利用するためのチュートリアルを提供します。
[このリポジトリ](https://github.com/0xhokusai/hokusai-get-started)をローカルに clone して、以下のチュートリアルへお進みください。

## チュートリアル
チュートリアルを始めるために、下記よりリポジトリをクローンしてください。
　
```bash
$ git clone https://github.com/0xhokusai/hokusai-get-started.git
```

インストールには、次の手順が必要です。
- Obtain API key
- Create your wallet
- Publish NFT metadata
- Access NFT metadata

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
    
    ![create_a_wallet.png](create_a_wallet.png)
    
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
    
    ![custom_rpc.png](custom_rpc.png)
    
3. それぞれのボックスに以下のように情報を入力してください。

    Property | Polygon Mainnet | Mumbai Testnet
    ---------|----------|---------
    ネットワーク名 | Polygon Mainnet | Mumbai Testnet
    新規 RPC URL | https://polygon-rpc.com | https://rpc-mubai.matic.today
    チェーン ID | 137 | 80001
    通貨記号(オプション) | MATIC | MATIC
    ブロックエクスプローラーのURL(オプション) | https://polygonscan.com | https://mumbai.polygonscan.com
    
    ![new_network_mainnet.png](new_network_mainnet.png)
    
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
#Hokusai API Keyを入力します。
HOKUSAI_API_KEY = "your-hokusai-api-key"
#Hokusai Contract IDを入力します。
HOKUSAI_CONTRACT_ID = "your-contract-id"
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

## 6. Hokusai API を動かす

Hokusai API を動かす準備が整いました。

### 6.1. NFT を発行する

NFT を Mint してみましょう。
以下のコードを実行してください。

{to}には発行したNFTを受け取るウォレットアドレスを指定します。このチュートリアルでは自分のウォレットアドレスを指定しましょう。

```bash
$ yarn mint-nft {to} {tokenUri}
```

パラメータの詳細については、[Hokusai API ドキュメント](../../swagger.yaml#mint-a-new-nft) を確認してください。

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
    
    ![account_details.png](account_details.png)
    
4. **秘密鍵のエクスポート**を押してください。
5. Metamaskのパスワードを入力し、確認を押してください。
6. 秘密鍵をコピーして**完了**を押してください。

`.env`の`WALLET_PRIVATE_KEY`に取得した秘密鍵を設定してください。

```bash
#Private key for your account
WALLET_PRIVATE_KEY = "your-private-key"
#API key for nft.storage
NFT_STORAGE_API_KEY = "your-nft-storage-api-key"
#Hokusai API key
HOKUSAI_API_KEY = "your-hokusai-api-key"
#Hokusai contract id
HOKUSAI_CONTRACT_ID = "your-contract-id"
```

下記のコードを実行してください。`{to}`にはNFT送信先のアカウントアドレスを指定します。

```bash
$ yarn transfer-nft {to} {tokenId}
{
  txHash: '0xdec77ee7148dc796dd08d656a256e1466daf2763c08cfe104f76e8baf318f3ed' # example Transaction Hash
}
```
> Metamask等ウォレットサービスに対応した場合は送信元ウォレットのprivate keyの管理をする必要がありません。

> private Keyは非常に重要な情報です。他の誰とも共有しないでください。

### 6.4. NFTを消却する  
NFTの消却を行う際は、0x000...にNFTを送信する必要があります。
詳しくは[こちら](docs/ja/burn.md)のドキュメンとご覧ください。


1. `.env`に送信元のウォレットのprivate keyを入力します。 

2. 下記コードを実行します。


```bash
$ yarn transfer-nft 0x0000000000000000000000000000000000000000 {tokenId}
{
  txHash: '0xdec77ee7148dc796dd08d656a256e1466daf2763c08cfe104f76e8baf318f3ed' # example Transaction Hash
}
```

これで、Hokusai API のチュートリアルは終了です。
Hokusai API を利用して、NFT を楽しみましょう！
