# Mint/BatchMintの実装を行うサンプル

ここでは、NFTをネットワーク上にMintするためのAPIであるMint/BatchMintの実装例について解説していきます。

version1ではNFTを１つずつしかMintできませんでした。しかしversion2では複数のメタデータを同時にMintできるように機能が拡張されました。このような拡張版のMintをBatchMintと呼んでいます。こちらではversion2の説明を行います。

以下に示すコードは[こちら](https://github.com/0xhokusai/hokusai-get-started/blob/main/src/v2/mintNft.ts)のget-startedのv2を参考にしています。

## NFTメタデータを公開する

[こちら](https://docs.hokusai.app/docs/hokusai-api/ZG9jOjIyMDIxMDI0-#5-nft%E3%81%AE%E3%83%A1%E3%82%BF%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B)を参考に事前にNFTメタデータを公開しておいてください。また、公開したメタデータのURLはメモしておきます。

## Mint/BatchMintに必要なデータをJSONで作成する

`address`には自身のmetamaskから確認できるウォレットアドレスを指定してください。

（Metamaskの設定がまだの場合[こちら](https://docs.hokusai.app/docs/hokusai-api/ZG9jOjIyMDIxMDI0-#2-%E3%82%A6%E3%82%A9%E3%83%AC%E3%83%83%E3%83%88%E3%82%92%E7%94%A8%E6%84%8F%E3%81%99%E3%82%8B)の対応をお願いいたします）

`tokenURI`には事前にNFTメタデータを公開した際に発行されたURLを貼り付けてください。

ひとつだけの場合（**Mint**）
`mint.json`

```json
[
  {
    "address": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  }
]
```

複数の場合（**BatchMint**）
`mint.json`

```json
[
  {
    "address": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  },
  {
    "address": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  },
  {
    "address": "0x11aaa11AAa111aa1a11111111A1111A11111111A",
    "tokenURI": "https://dweb.link/ipfs/xxxxxxxxxxxxxxxxxxxxxxxxxxx/metadata.json"
  }
]
```

## その他必要な値の設定

引数として使用したい重要な値は`.env`ファイルで設定をしておきます。

`HOKUSAI_API_KEY`と`HOKUSAI_CONTRACT_ID`と`CONTRACT_VERSION`の取得方法は[こちら](https://docs.hokusai.app/docs/hokusai-api/ZG9jOjIyMDIxMDI0-#1-api-key%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B)を参照してください。送られてくるメールに記載されています。

```
HOKUSAI_API_KEY = ""
HOKUSAI_CONTRACT_ID = ""
CONTRACT_VERSION = ""
```

## **Mint/BatchMintの実装**

ここでは上記で設定した値を利用して最終的に以下のような関数でMintを実行できるようにしたいと思います。

MintするデータはJSONで指定したものを読み込みます。

networkの指定は自身の選択したものにしてください。[こちら](https://docs.hokusai.app/docs/hokusai-api/ZG9jOjQ1MjUwNjM2-)から確認できます。ここではテストネットの方の`polygon-mumbai`を指定します。

**Typescript**

```tsx
import fetch from "node-fetch";
import mintBody from "./mint.json";
require("dotenv").config();
const env = process.env
const baseUrl = "https://mumbai.hokusai.app";
const network = "polygon-mumbai" // or polygon-mainnet
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

**Golang**

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
  baseUrl := "https://mumbai.hokusai.app"
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

### **Mint実行関数の定義**

まずは`mintNft`関数を定義します。

ここでは処理に必要な値を引数で定義するところまで行います。

**Typescript**

```tsx
interface mintBody {
  address: string;
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

**Golang**

```go
type MintBody struct {
  Address  string `json:"address"`
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

### リクエスト先URLの設定

`baseUrl`と`contractId`を利用してリクエスト(POST)を行うURLを`url`変数として定義する。

**Typescript**

```tsx
interface mintBody {
  address: string;
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

**Golang**

```go
type MintBody struct {
  Address  string `json:"address"`
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

### URLパラメータの付与

`?key=apiKey`を`baseUrl`+`/v2/${network}/nft/${contractVer}/${contractId}/mint`に付与するための処理を追加。

**Typescript**

```tsx
interface mintBody {
  address: string;
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

**Golang**

`http.NewRequest`関数で先にリクエストオブジェクトを生成しているが内容は次の手順で指定する。

```go
type MintBody struct {
  Address  string `json:"address"`
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

### POSTリクエスト処理の実装

引数の`body`を渡してPOSTします。

ここまで実装して、あとは引数に適当な値を入れて実行すればNFTをもう発行（**Mint**）できます！

**Typescript**

```tsx
interface mintBody {
  address: string;
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

**Golang**

```go
type MintBody struct {
  Address  string `json:"address"`
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

## 発行されたNFTがネットワーク上にあるのを確認する

今回発行したNFTは、以下のどちらかのネットワーク上にあるはずです。

自分で選んだ方のURLを選択してPolygonscanのwebサイトに飛んでください。

- Mumbai テストネット：[https://mumbai.polygonscan.com](https://mumbai.polygonscan.com/)

- Polygon メインネット：[https://polygonscan.com](https://polygonscan.com/)

![13B6F730-CD85-4BED-AFE9-6BB1023B4D13_1_105_c.jpeg](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/GTUknOfQXT8)

Polygonscanとは、Polygonネットワーク上に記録された全てのトランザクション、ブロック、アカウントの詳細を閲覧することができるウェブサイトです。

mintのレスポンスとして返却された`txHash`をサーチボックスにコピーしエンターを押すと、MintしたNFTの情報をみることができます。

![68297651-E651-4780-90D7-DC71A01DB9E5_1_105_c.jpeg](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/LgN6lZxTjA8)

[こちら](https://docs.hokusai.app/docs/hokusai-api/ZG9jOjIyMDIxMDI0-#611-polygonscan%E3%81%A7tokenid%E3%82%92%E7%A2%BA%E8%AA%8D%E3%81%99%E3%82%8B)も参考にしてください。
