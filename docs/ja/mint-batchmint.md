# Mint/BatchMintの実装を行うサンプル

ここでは、NFTをネットワーク上にMintするためのAPIであるMint/BatchMintの実装例について解説していきます。

version1ではNFTを１つずつしかMintできませんでした。しかしversion2では複数のメタデータを同時にMintできるように機能が拡張されました。このような拡張版のMintをBatchMintと呼んでいます。こちらではversion2の説明を行います。

以下に示すコードは[Github](https://github.com/0xhokusai/hokusai-get-started/blob/main/src/v2/mintNft.ts)のget-startedのv2を参考にしています。

## NFTメタデータを公開する

[5. NFTのメタデータを公開する](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDI0-hokusai#5-nft%E3%81%AE%E3%83%A1%E3%82%BF%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B)を参考に事前にNFTメタデータを公開しておいてください。また、公開したメタデータのURLはメモしておきます。

## Mint/BatchMintに必要なデータをJSONで作成する

`to`には自身のmetamaskから確認できるウォレットアドレスを指定してください。

（Metamaskの設定がまだの場合[2. ウォレットを用意する](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDI0-hokusai#2-ウォレットを用意する)より対応をお願いいたします）

`tokenURI`には事前に[NFTメタデータを公開](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDI0-hokusai#5-nftのメタデータを公開する)した際に発行されたURLを貼り付けてください。

<!--
type: tab
title: Mint
-->

ひとつだけの場合（**Mint**）
`mint.json`

```json
[
  {
    "to": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  }
]
```
<!--
type: tab
title: BatchMint
-->

複数の場合（**BatchMint**）
`mint.json`

```json
[
  {
    "to": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  },
  {
    "to": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  },
  {
    "to": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  }
]
```
<!-- type: tab-end -->


## その他必要な値の設定

引数として使用したい重要な値は`.env`ファイルで設定をしておきます。

- `HOKUSAI_API_KEY`
- `HOKUSAI_CONTRACT_ID`
- `CONTRACT_VERSION`
取得方法は[1. API Keyを取得する](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDI0-hokusai#1-api-key%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)を参照してください。送られてくるメールに記載されています。

```
HOKUSAI_API_KEY = ""
HOKUSAI_CONTRACT_ID = ""
CONTRACT_VERSION = ""
```

## **Mint/BatchMintの実装**

ここでは上記で設定した値を利用して最終的に以下のような関数でMintを実行できるようにしたいと思います。

MintするデータはJSONで指定したものを読み込みます。

networkの指定は自身の選択したものにしてください。[ネットワーク（チェイン）](https://docs.hokusai.app/docs/hokusai/ZG9jOjQ1MjUwNjM2-)から確認できます。ここではテストネットの方の`polygon-mumbai`を指定します。

次の章からは`mintNft（）`の実装の説明をしていきます。

<!--
type: tab
title: Typescript
-->

```tsx
import fetch from "node-fetch";
import mintBody from "./mint.json";
require("dotenv").config();
const env = process.env
const baseUrl = "https://api.hokusai.app";
const network = "polygon-mumbai" // or other networks
mintNft(
  baseUrl, 
  network,
  env.HOKUSAI_API_KEY, 
  env.CONTRACT_VERSION,
  env.HOKUSAI_CONTRACT_ID, 
  mintBody
)
.then((res) => {
  console.log(res);
})
.catch((err) => {
  console.log(err);
});
```

<!--
type: tab
title: Golang
-->

```go
package main

import (
  "encoding/json"
  "fmt"
  "go-hokusai-mint-sample/mint" // mint package内でMintNft関数を作成する
  "io/ioutil"
  "os"

  "github.com/joho/godotenv"
)

func main() {
  err := godotenv.Load(".env")
  if err != nil {
    fmt.Printf("読み込み出来ませんでした: %v", err)
  }
  apiKey := os.Getenv("HOKUSAI_API_KEY")
  contractVer := os.Getenv("HOKUSAI_CONTRACT_VERSION")
  contractId := os.Getenv("HOKUSAI_CONTRACT_ID")
  baseUrl := "https://api.hokusai.app"
  network := "polygon-mumbai"

  raw, err := ioutil.ReadFile("./batchmint.json")
  if err != nil {
    fmt.Println(err.Error())
    os.Exit(1)
  }
  var mintJson []mint.MintBody
  json.Unmarshal(raw, &mintJson)

  mint.MintNft(
    baseUrl,
    network,
    apiKey,
    contractVer,
    contractId,
    mintJson,
  )
}
```

<!-- type: tab-end -->


### **Mint実行関数の定義**

まずは`mintNft`関数を定義します。

次の章では実際にひとつずつ処理を関数内に追記していきます。

`mintBody`をinterfaceとして定義しているのはどういった形式でリクエストをなげればいいのかをわかりやすくするためです。

<!--
type: tab
title: Typescript
-->

```tsx
interface mintBody {
  to: string;
  tokenURI: string;
}
const mintNft = async (
  baseUrl: string,
  network: string,
  apiKey: string,
  contractVer: string,
  contractId: string,
  body: mintBody[]
) => {
  // ここに処理を書いていく
})
```

<!--
type: tab
title: Golang
-->

```go
type MintBody struct {
  To  string `json:"to"`
  TokenURI string `json:"tokenURI"`
}

func MintNft(
  baseUrl string,
  network string,
  apiKey string,
  contractVer string,
  contractId string,
  body []MintBody,
) error {
  // ここに処理を書いていく
}
```
<!-- type: tab-end -->

### リクエスト先URLの設定

`baseUrl`と`contractId`を利用してリクエスト(POST)を行うURLを`url`変数として定義する。

<!--
type: tab
title: Typescript
-->

```tsx
interface mintBody {
  to: string;
  tokenURI: string;
}
const mintNft = async (
  baseUrl: string,
  network: string,
  apiKey: string,
  contractVer: string,
  contractId: string,
  body: mintBody[]
) => {
  // リクエスト先URLを引数から生成
  const path = `/v2/${network}/nft/${contractVer}/${contractId}/mint`;
  const url = new URL(baseUrl + path);
})
```

<!--
type: tab
title: Golang
-->

```go
type MintBody struct {
  To  string `json:"to"`
  TokenURI string `json:"tokenURI"`
}

func MintNft(
  baseUrl string,
  network string,
  apiKey string,
  contractVer string,
  contractId string,
  body []MintBody,
) error {
  // リクエスト先URLを引数から生成
  path := "/v2/" + network + "/nft/" + contractVer + "/" + contractId + "/mint"
  url := baseUrl + path
}
```
<!-- type: tab-end -->

### URLパラメータの付与

`?key=apiKey`を`baseUrl`+`/v2/${network}/nft/${contractVer}/${contractId}/mint`に付与するための処理を追加。

<!--
type: tab
title: Typescript
-->

```tsx
interface mintBody {
  to: string;
  tokenURI: string;
}
const mintNft = async (
  baseUrl: string,
  network: string,
  apiKey: string,
  contractVer: string,
  contractId: string,	
  body: mintBody[]
) => {
  const path = `/v2/${network}/nft/${contractVer}/${contractId}/mint`;
  const url = new URL(baseUrl + path);
  // urlのパラメータに ?key=apiKey を追加している。
  const params = { key: apiKey };
  url.search = new URLSearchParams(params).toString();
}
```

<!--
type: tab
title: Golang
-->

`http.NewRequest`関数で先にリクエストオブジェクトを生成しているが内容は次の手順で指定する。

```go
type MintBody struct {
  To  string `json:"to"`
  TokenURI string `json:"tokenURI"`
}

func MintNft(
  baseUrl string,
  network string,
  apiKey string,
  contractVer string,
  contractId string,
  body []MintBody,
) error {
  path := "/v2/" + network + "/nft/" + contractVer + "/" + contractId + "/mint"
  url := baseUrl + path
  req, newReqErr := http.NewRequest(/* ここの引数は次の手順で指定します */)
  if newReqErr != nil {
	  return newReqErr
  }
  // urlのパラメータに ?key=apiKey を追加している。
  q := req.URL.Query()
  q.Add("key", apiKey)
  req.URL.RawQuery = q.Encode()
}
```
<!-- type: tab-end -->

### POSTリクエスト処理の実装

引数の`body`を渡してPOSTします。

ここまで実装して、あとは引数に適当な値を入れて実行すればNFTをもう発行（**Mint**）できます！

<!--
type: tab
title: Typescript
-->

```tsx
interface mintBody {
  to: string;
  tokenURI: string;
}
const mintNft = async (
  baseUrl: string,
  network: string,
  apiKey: string,
  contractVer: string,
  contractId: string,
  body: mintBody[]
) => {
  const path = `/v2/${network}/nft/${contractVer}/${contractId}/mint`;
  const url = new URL(baseUrl + path);
  const params = { key: apiKey };
  url.search = new URLSearchParams(params).toString();
  // 以下でPOST処理を行う
  const res = await fetch(url.toString(), {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(body),
  });
  // 失敗したときにエラーを返すようにしている。
  if (res.status != 200) {
    console.log(res.status);
    throw new Error(await res.text());
  }
  return res.json();
})
```

<!--
type: tab
title: Golang
-->

```go
type MintBody struct {
  To  string `json:"to"`
  TokenURI string `json:"tokenURI"`
}

func MintNft(
  baseUrl string,
  network string,
  apiKey string,
  contractVer string,
  contractId string,
  body []MintBody,
) error {
  path := "/v2/" + network + "/nft/" + contractVer + "/" + contractId + "/mint"
  url := baseUrl + path
// 構造体のbodyをjsonに変換
  jsonString, marshalErr := json.Marshal(body)
  if marshalErr != nil {
	  return marshalErr
  }
// 引数にbodyをjsonで指定
  req, newReqErr := http.NewRequest("POST", url, bytes.NewBuffer(jsonString))
  if newReqErr != nil {
	  return newReqErr
  }
// jsonでリクエストするためにCotent-Typeを指定
  req.Header.Add("Cotent-Type", `application/json"`)
  q := req.URL.Query()
  q.Add("key", apiKey)
  req.URL.RawQuery = q.Encode()
// リクエスト送信のためのClientを生成＆実行
  var client *http.Client = &http.Client{}
  res, err := client.Do(req)
  if err != nil {
	  log.Fatalln(err)
  }
  fmt.Println("res >>>>", res)
  return res
}
```
<!-- type: tab-end -->

## 発行されたNFTがネットワーク上にあるのを確認する

今回発行したNFTは、ブロックエクスプローラーから確認できます。

サンプルの通り `polygon-mumbai` を指定した場合は、[こちら](https://mumbai.polygonscan.com/)になります。

![13B6F730-CD85-4BED-AFE9-6BB1023B4D13_1_105_c.jpeg](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/GTUknOfQXT8)

Polygonscanとは、Polygonネットワーク上に記録された全てのトランザクション、ブロック、アカウントの詳細を閲覧することができるウェブサイトです。

mintのレスポンスとして返却された`txHash`をサーチボックスにコピーしエンターを押すと、MintしたNFTの情報をみることができます。

![68297651-E651-4780-90D7-DC71A01DB9E5_1_105_c.jpeg](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/LgN6lZxTjA8)

[Hokusaiを始める](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDI0-hokusai)も参考にしてください。
