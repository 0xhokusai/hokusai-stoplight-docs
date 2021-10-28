# Royalty

Royalty, in the NFT context, is a mechanism to pay the minter a percentage of the sale price each time your NFT is resold to a third party. NFTs minted using Hokusai API have a royalty setting. This enables the minter to easily receive the royalty when your NFTs are sold on a marketplace which supports sales with royalty.

We have a dedicated endpoint [`POST /nfts/{contractId}/{tokenId}/royalty`](../../swagger.yaml#get-royalty-of-the-NFT) for you to set royalty. You need two parameters `percentage` and `receiver` to call the endpoint to easily set the royalty for your NFT.


