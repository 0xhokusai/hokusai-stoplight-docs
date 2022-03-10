# DiscordのNFTメンバーシップ設定

### Discordのチャンネルやサーバーでメンバーシップとして使えるNFTを作ってみましょう。

NFTメンバーシップの例:

- 一部の大規模なNFTコレクション (例：[Bored Ape Yacht Club](https://opensea.io/collection/boredapeyachtclub)) は、トークンのホルダーにプライベートなコミュニティへのアクセス権が与えられます。
- より小規模なNFTコレクション (例：[Shiny Magpies](https://opensea.io/collection/shiny-magpies)) は、ホルダーに既存の課金者向けコミュニティへの生涯アクセス権が与えられます。

## NFTを発行する

Hokusaiを使ったNFT発行のより詳細な解説は[チュートリアル](get-started.md)をご覧ください。

### 1. API Keyを取得する

[こちら](https://0xhokusai.notion.site/Hokusai-API-Application-form-a6d8118d416b41d88632396e3156cddb)のフォームからAPI Keyの発行申請を行ってください。


### 2. nft.storageのAPI Keyを取得する

[ここ](https://nft.storage/login/)から[nft.storage](http://nft.storage)に登録してください。**API Keys** > **+ New Key**からAPI Keyを取得してください。


### 3. サンプルプロジェクトをクローンする

Hokusaiを使って簡単にNFTを発行するために、以下のコマンドでサンプルリポジトリをクローンしてください。

```bash
$ git clone https://github.com/0xhokusai/hokusai-get-started.git
```

### 4. 設定ファイルを作成する

以下のコマンドを実行して、`.env.sample`をコピーして`.env`ファイルを作成してください。

```bash
$ cp .env.sample .env
```

`.env`で環境変数を設定します。今回`WALLET_PRIVATE_KEY`は設定の必要はありません。

```bash
# 設定する必要はありません。
WALLET_PRIVATE_KEY = ""
# nft.storage API Keyを入力します。
NFT_STORAGE_API_KEY = "your-nft-storage-api-key"
# Hokusai API Keyを入力します。
HOKUSAI_API_KEY = "your-hokusai-api-key"
# Hokusai Contract IDを入力します。
HOKUSAI_CONTRACT_ID = "your-contract-id"
# Hokusai Contract Addressを入力します。
HOKUSAI_CONTRACT_ADDRESS = "your-contract-address"
```

### 5. 必要なパッケージをインストールする

以下のコマンドを実行して必要なパッケージのインストールを行います。

```bash
$ yarn
```

### 6. NFTのメタデータを公開する

NFTを発行する前に、nft.storageを利用してサンプルデータをNFTのメタデータとしてアップロードします。アップロードするメタデータは`src/storeMetadata.ts`から編集することができます。以下のコードを実行してください。

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

### 7. NFTを発行する

以下のコマンドを実行して、NFTを発行します。

```bash
$ yarn mint-nft {to} {tokenUri}
{
  txHash: '0x8765feaa11a7e0f9f4a84f21415434d80dd9be27728a8f6eff4d402e4d0c2766' # example Transaction Hash
}
```
引数の詳細については、[リファレンス](../../reference/swagger-v2.yaml#mints-new-nft) を参照してください。

## Discordでroleを設定する

[collab.land](https://collab.land/)のDiscord botを使ってメンバーシップを設定します。詳細は[こちら](https://collabland.freshdesk.com/support/solutions/articles/70000036689-discord-bot-walkthrough)。

1. [招待リンク](https://discord.com/oauth2/authorize?client_id=704521096837464076&scope=bot&permissions=8)から、サーバーにbotを招待します。
2. **#collabland-config**チャンネルで`!setup role`と送信して、roleの設定に進みます。
3. `MATIC`を選択します。
4. アクセス権を付与したいNFTの詳細を入力していきます。
    1. `contract address`は[管理画面](https://dashboard.hokusai.app/)の`Contracts`ページから確認できます。
    2. `token id`は[Polygonscan](https://polygonscan.com/)から、`txHash`か、`contract address`を使って確認することができます。詳細な手順は[こちら](get-started.md#611-polygonscanでtokenidを確認する)から。


## 🎊 以上で、NFTホルダーがプライベートチャンネルにアクセスできるようになりました！

新たなメンバーがDiscordに参加したら、`!join`コマンドを使ってcollab.landの設定をしてもらいましょう。完了後、設定したNFTをもつメンバーは指定されたチャンネルに参加することができるようになります。
