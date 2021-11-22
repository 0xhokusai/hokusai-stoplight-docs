# Royalty

Royalty, in the NFT context, is a mechanism to pay the minter a percentage of the sale price each time your NFT is resold to a third party. Contracts deployed with Hokusai API implement [EIP-2981](https://eips.ethereum.org/EIPS/eip-2981), which is an ethereum standard to promise a contract to manage royalty information.

We have a dedicated endpoint [`POST /nfts/{contractId}/{tokenId}/royalty`](../../swagger.yaml#get-royalty-of-the-NFT) for you to set royalty. You need two parameters `percentage` and `receiver` to call the endpoint to easily set the royalty for your NFT.



## Set up royalty for OpenSea
Although EIP-2981 was proposed to enable royalty payments to happen across different NFT marketplaces, major marketplaces like OpenSea and Rarible are yet to support EIP-2981 and they each have unique royalty implementations. We will explain how to list your NFTs and set royalty in OpenSea in this section.

### List NFT on OpenSea
List your NFT [here](https://opensea.io/get-listed/step-two) by copy & pasting your contract address. 