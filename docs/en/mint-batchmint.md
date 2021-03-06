# Sample to implement Mint/BatchMint

This section describes an example implementation of Mint/BatchMint, an API for minting NFTs on a network.

The code shown below is based on v2 of get-started from [Github](https://github.com/0xhokusai/hokusai-get-started/blob/main/src/v2/mintNft.ts)

## Publish NFT metadata

[5. Publish NFT metadata](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDEx-getting-started-with-hokusai#5-publish-nft-metadata) for reference. Also, note the URL of the published metadata.

## Create the data needed for Mint/BatchMint in JSON

The `to` should be a wallet to that can be verified from your own Metamask.

If you have not yet set up Metamask [2. Prepare a wallet](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDEx-getting-started-with-hokusai#2-prepare-a-wallet)

In the `tokenURI` field, paste the URL that was issued when you published the NFT metadata in advance.

<!--
type: tab
title: Mint
-->
When only one（**Mint**）
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
Multiple cases（**BatchMint**）
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

## Set other necessary values

Important values to be used as arguments are set in the `.env` file.

How to get `HOKUSAI_API_KEY`, `HOKUSAI_CONTRACT_ID` and `CONTRACT_VERSION` is [1. Obtain your API key](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDEx-getting-started-with-hokusai#1-obtain-your-api-key). You will find this information in the email you receive.

```
HOKUSAI_API_KEY = ""
HOKUSAI_CONTRACT_ID = ""
CONTRACT_VERSION = ""
```

## **Mint/BatchMint implementation**

Here we would like to be able to use the values set above to finally execute Mint with the following function.

The data to be minted is read as specified in JSON.

The network designation should be your own choice. You can check it [Network (chain)](https://docs.hokusai.app/docs/hokusai/ZG9jOjQ1MjUwNjM1-network-chain). Here we specify `polygon-mumbai` for the test net.

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
  "go-hokusai-mint-sample/mint" // Create MintNft function in mint package
  "io/ioutil"
  "os"

  "github.com/joho/godotenv"
)

func main() {
  err := godotenv.Load(".env")
  if err != nil {
    fmt.Printf("Could not load.: %v", err)
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

### **Define Mint Execution Functions**

First, define the `mintNft` function.

This section goes as far as defining the values needed for processing with arguments.

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
  // I'll write the process here.
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
  // I'll write the process here.
}
```
<!-- type: tab-end -->

### Setting the URL to request

Define the URL to make the request (POST) using `baseUrl` and `contractId` as the `url` variable.

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
  // Generate request URL from arguments
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
  // Generate request URL from arguments
  path := "/v2/" + network + "/nft/" + contractVer + "/" + contractId + "/mint"
  url := baseUrl + path
}
```
<!-- type: tab-end -->

### Assign URL parameters

Added process to grant `?key=apiKey` to `baseUrl`+`/v2/${network}/nft/${contractVer}/${contractId}/mint`.

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
  // Adding "?key=apiKey" to the url parameter.
  const params = { key: apiKey };
  url.search = new URLSearchParams(params).toString();
}
```

<!--
type: tab
title: Golang
-->

The `http.NewRequest` function creates the request object first, but the contents are specified in the following steps.

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
  req, newReqErr := http.NewRequest(/* The argument here is specified in the following steps */)
  if newReqErr != nil {
	  return newReqErr
  }
  // Adding "?key=apiKey" to the url parameter.
  q := req.URL.Query()
  q.Add("key", apiKey)
  req.URL.RawQuery = q.Encode()
}
```
<!-- type: tab-end -->

### Implementation of POST request processing

POST passing `body` argument.

Now that you've implemented this far, all you need to do is to run it with the appropriate values in the arguments, and you can already issue (**Mint**) the NFT!

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
  // POST processing under
  const res = await fetch(url.toString(), {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(body),
  });
  // Returns an error on failure
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
// Convert structure body to json
  jsonString, marshalErr := json.Marshal(body)
  if marshalErr != nil {
	  return marshalErr
  }
// Specify body as json argument
  req, newReqErr := http.NewRequest("POST", url, bytes.NewBuffer(jsonString))
  if newReqErr != nil {
	  return newReqErr
  }
// Cotent-Type to request in json
  req.Header.Add("Cotent-Type", `application/json"`)
  q := req.URL.Query()
  q.Add("key", apiKey)
  req.URL.RawQuery = q.Encode()
// Generate & execute Client for sending request
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

## Verify that the issued NFT is on the network

You can check the NFT issued this time from Block Explorer.

If you are using `polygon-mumbai` as sampled, you can check [Polygonscan](https://mumbai.polygonscan.com/) (Mumbai).

![13B6F730-CD85-4BED-AFE9-6BB1023B4D13_1_105_c.jpeg](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/GTUknOfQXT8)

Polygonscan is a website that allows you to view details of all transactions, blocks, and accounts recorded on the Polygon network.

Copy the `txHash` returned as a mint response into the search box and press enter to see the information of the NFT you minted.

![68297651-E651-4780-90D7-DC71A01DB9E5_1_105_c.jpeg](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/LgN6lZxTjA8)

Please refer to [Getting Started with Hokusai](https://docs.hokusai.app/docs/hokusai/ZG9jOjIyMDIxMDEx-getting-started-with-hokusai)
