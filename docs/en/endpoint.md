# Endpoint
Hokusai API is sending requests to the following endpoint. The base endpoint is:
```
testnet: https://mumbai.hokusai.app  
mainnet: https://polygon.hokusai.app  
```

Currently the Hokusai API provides the following endpoints.

If you using the Mumbai testnet, change the baseUrl of `src/getNft.ts` and `src/mintNft.ts`'s to "https://mumbai.hokusai.app".


|Endpoint|Description|
|--|--|
|[`GET /v1/nfts/{contractId}/{tokenId}`](../../swagger.yaml#get-information-of-the-nft)|Get NFT information|
|[`POST /v1/nfts/{contractId}/mint`](../../swagger.yaml#)|[Mint](glosarry.md#Mint) an NFT|
|[`POST /v1/nfts/{contractId}/transfer`](../../swagger.yaml#transfer-a-nft-with-meta-transaction)|Transfer an NFT|
|[`GET /v1/nfts/{contractId}/{tokenId}/royalty`](../../swagger.yaml#get-royalty-of-the-nft)|Get a royalty of a specific NFT|
|[`POST /v1/nfts/{contractId}/{tokenId}/royalty`](../../swagger.yaml#set-royalty-to-the-nft)|Set a royalty of a specific NFT|
