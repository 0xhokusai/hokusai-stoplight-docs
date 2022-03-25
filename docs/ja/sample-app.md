## Transferを行うサンプル

このページでは、Hokusaiを利用して、ガス代なし（Meta Transaction）でNFTをTransferをするサンプルを紹介します。
この機能を実装すれば、実装したサービスを利用するユーザが、ガス代を負担することなく、サービスを利用することができるようになります。（つまり**暗号資産を持たないユーザ向けのサービスを展開**することができます）

今回使うサンプルコードは[こちら](https://github.com/0xhokusai/hokusai-api-client-sample)となります。実際に動作しているアプリは[こちら](https://client.hokusai.app)からご利用できます。

![sample-app.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/fWSLpOFidWE)


## API Keyを発行する
HokusaiではTestnet用のAPI Keyを無料で配布しています。
以下の[フォーム](https://www.notion.so/0xhokusai/Plolygon-Testnet-42bda92114ef4c28833e38fbc6fa04e0)から利用申請を行うことができます。


## Metamaskの実装
今回はWalletとして、一般的である[Metamask](https://docs.metamask.io/guide/#why-metamask
)を利用します。

### ネットワーク設定
今回はPolygonのTestnetであるMumbaiを利用するので、以下のようにネットワークを設定します。
```Typescript
const networkParam = {
  chainId: '0x13881',
  chainName: 'Mumbai Testnet',
  nativeCurrency: { name: 'Matic', symbol: 'MATIC', decimals: 18 },
  rpcUrls: ['https://rpc-mumbai.matic.today'],
  blockExplorerUrls: ['https://mumbai.polygonscan.com/'],
};
```
注意するべき点としてchainIdは10進数ではなく**16進数の文字列**で設定する必要があります。
ネットワーク情報についてはPolygon公式の[こちらのサイト](https://docs.polygon.technology/docs/develop/network-details/network)に記述されています。


### Metamaskとの接続
続いて、Metamaskを接続しProviderを取得する関数を定義します。
`wallet_addEthereumChain`メソッドを利用することで、ネットワーク切り替えのモーダルを表示させることができます（ユーザがそのネットワークをMetamaskに登録してない場合、同時にネットワークを登録させることができます。）
```Typescript
import { ethers } from 'ethers';

async function connectMetamask() {
  // Initialize Metamask
  const provider = await window.ethereum
    .request({
      method: 'eth_requestAccounts',
      params: [{ eth_accounts: {} }],
    })
    .then(() => new ethers.providers.Web3Provider(window.ethereum))
    .catch((error: Error) => {
      console.log(error);
    });

  // Set network
  await window.ethereum.request({
    method: 'wallet_addEthereumChain',
    params: [networkParam],
  });
  return provider;
}
```

## HokusaiでNFTをTransferする
NFTをTransferする手順は以下の通りです。
1. Transactionのデータを作成
2. 1.のデータをMetamaskで署名
3. 1.で作成したデータと2.で作成した署名をHokusaiにPost

また、HokusaiでMeta Transactionを利用する際には、[`transfer`](../../reference/swagger-v2.yaml#transfer)エンドポイントを利用します。
Bodyに必要なデータは、以下の通りです。
- `from`: Transaction送信者のアドレス
- `to`: HokusaiのNFTコントラクトのアドレス
- `value`: 送金する金額（=0）
- `gas`: ガス
- `nonce`：Transaction送信者のNonce
- `data`：Transactionのデータ（NFTのコントラクトで実行するデータ）
- `signature`: ユーザがMetamaskで署名した`data`
```json
{
  "request": {
    "from": string,
    "to": string,
    "value": number,
    "gas": number,
    "nonce": number,
    "data": string
  }
}
```


### 1. Transactionデータの作成
Transactionのデータを作成します。
Transactionのデータ構造は以下の通りです。
```json
{
  from: string;
  to: string;
  value: number;
  gas: number;
  nonce: number;
  data: string;
};
```

まず初めに、`data`に入れるNFTのコントラクトで実行したい関数のデータを作成します
ABIは[こちら](https://github.com/0xhokusai/hokusai-api-client-sample/blob/main/src/abis/ERC721WithRoyaltyMetaTx.json)のものを使います。


```typescript
import { ethers } from 'ethers';
// https://github.com/0xhokusai/hokusai-api-client-sample/blob/main/src/abis/ERC721WithRoyaltyMetaTx.json
import HokusaiAbi from '../abis/ERC721WithRoyaltyMetaTx.json';

// 接続しているウォレットアドレスを取得
const signer = provider.getSigner();
const from = await signer.getAddress();

// ABIの読み込み
const hokusaiInterface = new ethers.utils.Interface(HokusaiAbi.abi);

// データの作成
const data = hokusaiInterface.encodeFunctionData('transferFrom', [
   from,
   toAddress,
   tokenId,
]);
```
`const hokusaiInterface = new ethers.utils.Interface(HokusaiAbi.abi);`でAbiを読み込み、`encodeFunctionData`で関数のデータを作成しています。
ここで、`encodeFunctionData`の引数は、
- `from`: ユーザのウォレットアドレス
- `toAddress`: NFTの移転先のウォレットアドレス
- `tokenId`: 移転したいNFTのID

となります。

続いて他のパラメータを設定します。
```typescript
import ForwarderAbi from '../abis/MinimalForwarder.json';

type Message = {
  from: string;
  to: string;
  value: number;
  gas: number;
  nonce: number;
  data: string;
};


// forwarderコントラクトの読み込み
const forwarder = new ethers.Contract(
  forwarderAddress, // 0x0E285b682EAF6244a2AD3b1D25cFe61BF6A41fc3 ← テストネット用
  ForwarderAbi.abi,
  provider
);

const message: Message = {
  from,
  to: contractAddress, // 0x73b5373a27f4a271c6559c6c83b10620acde9a2a ← テストネット用
  value: 0,
  gas: 1e6,
  nonce: (await forwarder.getNonce(from)).toNumber(),
  data,
};

```
`forwarderAddress`と`contractAddress`は以下のように設定します。

address | Mumbai Testnet | Polygon Mainnet 
--------|----------------|----------------
`forwarderAddress` | 0x0E285b682EAF6244a2AD3b1D25cFe61BF6A41fc3 | 0xD64a425d91a97866cE4ee2d759A23560411ADb01
`contractAddress` | 0x73b5373a27f4a271c6559c6c83b10620acde9a2a | ご自身のcontractAddress


また、`forwarder.getNonce(from)`でTransactionに必要なNonceをForwarderから取得します。
`data`は、先程`encodeFunctionData`で作成したものを利用します。


### 2. Metamaskでデータに署名
署名に必要な手順は以下の通りです。
1. 署名データを作成（先程作成した`message`を利用）
2. Metamaskの署名モーダルを表示し、ユーザに署名させる

#### 署名データの作成
[`signTypedData_v4`](https://docs.metamask.io/guide/signing-data.html#sign-typed-data-v4
)の署名データを作成します。


署名データを作成する関数`createTypedDataV4()`を定義します。
```typescript
const EIP712DomainType = [
  { name: 'name', type: 'string' },
  { name: 'version', type: 'string' },
  { name: 'chainId', type: 'uint256' },
  { name: 'verifyingContract', type: 'address' },
];

const ForwardRequestType = [
  { name: 'from', type: 'address' },
  { name: 'to', type: 'address' },
  { name: 'value', type: 'uint256' },
  { name: 'gas', type: 'uint256' },
  { name: 'nonce', type: 'uint256' },
  { name: 'data', type: 'bytes' },
];

// https://eips.ethereum.org/EIPS/eip-712
export function createTypedDataV4(
  chainId: number,
  ForwarderAddress: string,
  message: Message
) {
  const TypedData = {
    primaryType: 'ForwardRequest' as const,
    types: {
      EIP712Domain: EIP712DomainType,
      ForwardRequest: ForwardRequestType,
    },
    domain: {
      name: 'MinimalForwarder',
      version: '0.0.1',
      chainId,
      verifyingContract: ForwarderAddress,
    },
    message,
  };
  return TypedData;
}
```

次に、`createTypedDataV4()`を利用して、署名データを作成します。
`message`は1つ前で作成した`message`となります。
```typescript
const { chainId } = await provider.getNetwork();

const typedData = createTypedDataV4(
  chainId,
  forwarderAddress, // 0x0E285b682EAF6244a2AD3b1D25cFe61BF6A41fc3
  message
);
```
#### Metamaskの署名モーダルを表示し、ユーザに署名させる
署名したデータを`signature`という変数に格納しています。
```typescript
const signature = await provider.send('eth_signTypedData_v4', [
  from,
  JSON.stringify(typedData),
]);
```


### 3. Post!
最後にHokusaiに作成した`message`と`signature`をPostします。
`contractId`と`apiKey`は、ご自身のものを設定してください。

<!--
type: tab
title: v2
-->

```typescript
const network = 'polygon-mumbai' // polygon testnet
const contractVer = 'your-contract-version'
const contractId = 'your-contract-id'
const apiKey = 'your-api-key'

result = await fetch(
  `https://api.hokusai.app/v2/${network}/nft/${contractVer}/${contractId}/transfer?key=${apiKey}`,
  {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ request: { ...message, signature } }),
  }
).then((res) => res.json())
```

<!--
type: tab
title: v1
-->

```typescript
const contractId = 'your-contract-id'
const apiKey = 'your-api-key'

result = await fetch(
  `https://mumbai.hokusai.app/v1/nfts/${contractId}/transfer?key=${apiKey}`,
  {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ request: { ...message, signature } }),
  }
).then((res) => res.json())
```

<!-- type: tab-end -->

## まとめ
Hokusaiを利用して、ガス代なし（Meta Transaction）でNFTをTransferをするためには次の手順が必要です。

1. Transactionデータの作成
2. 署名データの作成＆Metamaskでの署名
3. HokusaiにPost