## はじめに

今回は、Hokusai APIを利用した以下の実装例を紹介します。

- Walletを接続したユーザが特定のNFTを持っているか検証
- Walletを接続したユーザが何個NFTをもっているか検証

## Hokusai APIとは

[Hokusai API](https://hokusai.app/)は、NFTのMint（発行）、Transfer（移転）、Burn（消却）、Royaltyの設定、Metadata情報の取得などの機能をAPIとして提供しているサービスです。
自分でコントラクトを書かずにNFTのサービスを作ることができます。

## API Keyを発行する

Hokusai APIではTestnet用のAPI Keyを無料で配布しています。
[このフォーム](https://ir9l8pcvcmm.typeform.com/to/xSbuj2WA)から利用申請を行うことができます。

## Metamaskの実装

今回はWalletとして、一般的である[Metamask](https://docs.metamask.io/guide/#why-metamask
)を利用します。

Metamaskでの実装については、[過去の記事](https://zenn.dev/hokusai/articles/aa40fd375cc523#metamask%E3%81%AE%E5%AE%9F%E8%A3%85)で紹介しています。

## ContractのABIの読み込み

コントラクト上の関数を呼び出すためにABIを読み込みます。
[こちら](https://github.com/0xhokusai/hokusai-api-client-sample/blob/main/src/abis/ERC721WithRoyaltyMetaTx.json)は、Hokusai APIで利用しているコントラクトのABIです。

`contractAddress`については、API申請時のメールまたは、[管理画面](https://dashboard.hokusai.app)に記載されています。

```Typescript
import HokusaiAbi from '../abis/ERC721WithRoyaltyMetaTx.json';

const contractAddress = 'API申請時のメールに記載されたコントラクトアドレス'
const hokusaiContract = new ethers.Contract(
  contractAddress, 
  HokusaiAbi.abi,
  provider // 前項のMetamaskの実装で取得したprovider
);
```

## Walletを接続したユーザが特定のNFTを持っているか検証

ある特定の`tokenId`のNFTの所有者と、Metamaskで接続しているアカウントのアドレスが一致するか確認します。

```Typescript
const signer = provider.getSigner(0); // Metamaskのアカウントを取得
const address = await signer.getAddress(); // Metamaskのアカウントのアドレスを取得
const tokenId = 1;
const tokenHolder = await contract.ownerOf(tokenId); // {tokenId}の所有者

const isHolder = tokenHolder === address; // NFT所有者とMetamaskのアカウントを比較
```


## Walletを接続したユーザが何個NFTをもっているか検証

Hokusai APIで発行したコントラクトはERC721Enumerableに対応しています。
これを利用して、そのコントラクトでMintされたNFTを所有しているユーザのみアクセスできるページなどを実装することができます。

```Typescript
const signer = provider.getSigner(0); // Metamaskのアカウントを取得
const address = await signer.getAddress(); // Metamaskのアカウントのアドレスを取得
const tokenBalance = await contract.balanceOf(address); // NFT所有量を取得
```
ここで`tokenBalance > 0`であると、そのコントラクトでMintされたNFTを所有しているユーザとなります。

## まとめ

今回は、Hokusai APIを利用した以下の実装例を紹介しました。

- `ownerOf(tokenId)`を利用して、Walletを接続したユーザが特定のNFTを持っているか検証
- `balanceOf(address)`を利用して、Walletを接続したユーザが何個NFTをもっているか検証

これらを利用して、そのコントラクトでMintされたNFTを所有しているユーザのみアクセスできるページなどを実装することができます。

## 宣伝

HokusaiはNFTの開発インフラ「Hokusai API」を提供しています。
「デジタル上で価値を流通させたい全ての個人・事業者にとってのインフラ」としてAPIサービスを提供するEmbedded NFTサービスです。

https://hokusai.app/

「Hokusai API」を提供する日本モノバンドル株式会社では、エンジニアを採用中です！
ぜひフルリモートで、スピード感や大きな変化を楽しみながらぜひ働いてみませんか？

[Hokusai 採用ページ](https://www.notion.so/0xhokusai/Backend-engineer-aabdbbbb48584113854e9e8102f13d6b)

## この記事を書いた人

<img src="https://storage.googleapis.com/zenn-user-upload/e9d420e447090c39b0d22af2.png" width=250)>

**Tarumi - CTO**

[https://y.at/🔥🍞🔥🍞🔥](https://y.at/%F0%9F%94%A5%F0%9F%8D%9E%F0%9F%94%A5%F0%9F%8D%9E%F0%9F%94%A5)




