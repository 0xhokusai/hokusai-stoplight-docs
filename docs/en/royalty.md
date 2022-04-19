# Royalty

Royalty, in the NFT context, is a mechanism to pay the minter a percentage of the sale price each time your NFT is resold to a third party. 

Contracts deployed with Hokusai implement [EIP-2981](https://eips.ethereum.org/EIPS/eip-2981), which is an ethereum standard to promise a contract to manage royalty information. This enables any marketplace that supports EIP-2981 to distribute the royalty fee to the minter.

We have a dedicated endpoint for you to set royalty. 

<!--
type: tab
title: v2
-->

[`POST /v2/{network}/nft/{contractVer}/{contractId}/{tokenId}/royalty`](../../reference/swagger-v2.yaml#set-royalty-to-the-NFT) 

<!--
type: tab
title: v1
-->

[`POST /v1/nfts/{contractId}/{tokenId}/royalty`](../../reference/swagger-v1.yaml#set-royalty-to-the-NFT) 

<!-- type: tab-end -->

You need two parameters `percentage` and `receiver` to call the API to easily set the royalty for your NFT.