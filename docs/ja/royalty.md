# ロイヤルティ

ロイヤルティとは、NFT を他人に譲渡する際に、NFT 発行者が売買価格の一部を受け取れるようにする仕組みです。

Hokusai API は、ethereumでのロイヤルティの標準規格である[EIP-2981](https://eips.ethereum.org/EIPS/eip-2981)に準拠したコントラクトを用いて NFT を発行します。これによって、このNFTがEIP-2981に対応するマーケットプレイス上で取引される度に設定したロイヤルティを受け取ることができます。

Hokusai API では、ロイヤルティを設定する [`POST /nfts/{contractId}/{tokenId}/royalty`](../../swagger.yaml#set-royalty-to-the-NFT) エンドポイントが存在しています。`percentage` と `receiver` という、２つのパラメータと共にエンドポイントを叩くことで、ロイヤルティの設定を簡単に行うことができます。

