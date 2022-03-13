# Endpoint
Hokusai is sending requests to the following endpoint. The base endpoint is:
```
testnet: https://mumbai.hokusai.app  
mainnet: https://polygon.hokusai.app  
```

Currently the Hokusai provides the following endpoints.

If you using the Mumbai testnet, change the baseUrl of `src/getNft.ts` and `src/mintNft.ts`'s to "https://mumbai.hokusai.app".


```
https://mumbai.hokusai.app
```

The endpoint varies from version to version and the current default is v2.  
The supported version differs for each smart contract, please check the version information on the email sent when the smart contract was created.

<!--
type: tab
title: v2
-->

|Endpoint|Description|
|--|--|
|[`GET /v2/{network}/nft/{contractVer}/{contractId}/{tokenId}`](../../reference/swagger-v2.yaml#get-information-of-the-nft)|Get NFT information|
|[`POST /v2/{network}/nft/{contractVer}/{contractId}/mint`](../../reference/swagger-v2.yaml#mints-new-nft)|[Mint](glossary.md#Mint) an NFT|
|[`POST /v2/{network}/nft/{contractVer}/{contractId}/transfer`](../../reference/swagger-v2.yaml#transfer-a-nft-with-meta-transaction)|Transfer an NFT|
|[`GET /v2/{network}/nft/{contractVer}/{contractId}/{tokenId}/royalty`](../../reference/swagger-v2.yaml#get-royalty-of-the-nft)|Get a royalty of a specific NFT|
|[`POST /v2/{network}/nft/{contractVer}/{contractId}/{tokenId}/royalty`](../../reference/swagger-v2.yaml#set-royalty-to-the-nft)|Set a royalty of a specific NFT|

<!--
type: tab
title: v1
-->

|Endpoint|Description|
|--|--|
|[`GET /v1/nfts/{contractId}/{tokenId}`](../../reference/swagger-v1.yaml#get-information-of-the-nft)|Get NFT information|
|[`POST /v1/nfts/{contractId}/mint`](../../reference/swagger-v1.yaml#mint-a-new-nft)|[Mint](glosarry.md#Mint) an NFT|
|[`POST /v1/nfts/{contractId}/transfer`](../../reference/swagger-v1.yaml#transfer-a-nft-with-meta-transaction)|Transfer an NFT|
|[`GET /v1/nfts/{contractId}/{tokenId}/royalty`](../../reference/swagger-v1.yaml#get-royalty-of-the-nft)|Get a royalty of a specific NFT|
|[`POST /v1/nfts/{contractId}/{tokenId}/royalty`](../../reference/swagger-v1.yaml#set-royalty-to-the-nft)|Set a royalty of a specific NFT|

<!-- type: tab-end -->
