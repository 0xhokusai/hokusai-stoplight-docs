# エンドポイント
Hokusai では以下のベース URL に対してリクエストを送信します。

```
https://api.hokusai.app  
```

また、Hokusai では現在、以下のようなエンドポイントを提供しています。

エンドポイントはバージョンごとに異なり、現在のデフォルトはv2です。  
コントラクトごとに対応しているバージョンが異なるため、コントラクト作成時に送られてくるemailに記載されるバージョン情報をご確認ください。

<!--
type: tab
title: v2
-->

|エンドポイント|説明|
|--|--|
|[`GET /v2/{network}/nft/{contractVer}/{contractId}/{tokenId}`](../../reference/swagger-v2.yaml#get-information-of-the-nft)|NFTの情報を取得する|
|[`POST /v2/{network}/nft/{contractVer}/{contractId}/mint`](../../reference/swagger-v2.yaml#mints-new-nft)|NFTを発行する|
|[`POST /v2/{network}/nft/{contractVer}/{contractId}/transfer`](../../reference/swagger-v2.yaml#transfer-a-nft-with-meta-transaction)|NFTを送信する|
|[`GET /v2/{network}/nft/{contractVer}/{contractId}/{tokenId}/royalty`](../../reference/swagger-v2.yaml#get-royalty-of-the-nft)|NFTのロイヤリティ情報を取得する|
|[`POST /v2/{network}/nft/{contractVer}/{contractId}/{tokenId}/royalty`](../../reference/swagger-v2.yaml#set-royalty-to-the-nft)|NFTのロイヤリティを設定する|

<!--
type: tab
title: v1
-->

|エンドポイント|説明|
|--|--|
|[`GET /v1/nfts/{contractId}/{tokenId}`](../../reference/swagger-v1.yaml#get-information-of-the-nft)|NFTの情報を取得する|
|[`POST /v1/nfts/{contractId}/mint`](../../reference/swagger-v1.yaml#mint-a-new-nft)|NFTを発行する|
|[`POST /v1/nfts/{contractId}/transfer`](../../reference/swagger-v1.yaml#transfer-a-nft-with-meta-transaction)|NFTを送信する|
|[`GET /v1/nfts/{contractId}/{tokenId}/royalty`](../../reference/swagger-v1.yaml#get-royalty-of-the-nft)|NFTのロイヤリティ情報を取得する|
|[`POST /v1/nfts/{contractId}/{tokenId}/royalty`](../../reference/swagger-v1.yaml#set-royalty-to-the-nft)|NFTのロイヤリティを設定する|

<!-- type: tab-end -->
