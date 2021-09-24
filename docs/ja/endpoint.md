# エンドポイント
Hokusai API では以下のベース URL に対してリクエストを送信します。

```
testnet: https://mumbai.hokusai.app  
mainnet: https://polygon.hokusai.app  
```

また、Hokusai API では現在、以下のようなエンドポイントを提供しています。
Currently the Hokusai API provides the following endpoints.

テストネットをご利用の場合は`src/getNft.ts`と`src/mintNft.ts`のbaseUrlを"https://mumbai.hokusai.app"に変更してください。

```
https://mumbai.hokusai.app
```



|エンドポイント|説明|
|--|--|
|[`GET /v1/nfts/{contractId}/{tokenId}`](../swagger.yaml#get-information-of-the-nft)|NFTの情報を取得する|
|[`POST /v1/nfts/{contractId}/mint`](../swagger.yaml#)|NFTを発行する|
|[`POST /v1/nfts/{contractId}/transfer`](../swagger.yaml#transfer-a-nft-with-meta-transaction)|NFTを送信する|
|[`GET /v1/nfts/{contractId}/{tokenId}/royalty`](../swagger.yaml#get-royalty-of-the-nft)|NFTのロイヤリティ情報を取得する|
|[`POST /v1/nfts/{contractId}/{tokenId}/royalty`](../swagger.yaml#set-royalty-to-the-nft)|NFTのロイヤリティを設定する|
