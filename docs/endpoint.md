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
|[`GET /v1/nfts/{contractId}/{tokenId}`](nft/get)|Get NFT information|
|[`POST /v1/nfts/{contractId}/mint`](nft/mint)|[Mint](glosarry.md#Mint) an NFT|
|[`GET /v1/nfts/{contractId}/{tokenId}/royalty`](royalty/get)|Get a royalty of a specific NFT|
|[`POST /v1/nfts/{contractId}/{tokenId}/royalty`](royalty/set)|Set a royalty of a specific NFT|