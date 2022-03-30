## Sample project: Implement Transfer

In this tutorial, we will guide you through steps to implement transfer using Hokusai. Transferring NFT with Hokusai costs you no gas as we use meta transaction, where Hokusai pays the gas for you. After reading through this tutorial, you will be able to launch a service where the users do not need to pay gas to perform transfer. (i.e. You can build a NFT platform particularly for **users who do not own crypto assets!**)

In this tutorial, we will use a [sample app](https://github.com/0xhokusai/hokusai-api-client-sample) as a guide. You can find the running app [here](https://client.hokusai.app).

![sample-app.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/fWSLpOFidWE)

## Get an API Key
Hokusai offers a free API Key for Testnet use. Register for an API Key from [here](https://www.notion.so/0xhokusai/Plolygon-Testnet-42bda92114ef4c28833e38fbc6fa04e0) and your API Key and Contract ID will be sent via email.

## Integrate Metamask to your app
In this tutorial, we will be using [Metamask](https://docs.metamask.io/guide/#why-metamask), one of the most standard wallet applications out there.

### Set up network
We will be using Mumbai which is a testnet for Polygon. Configure network information as follows.
```Typescript
// src/context/WalletProvider.tsx
const networkParam = {
  chainId: '0x13881',
  chainName: 'Mumbai Testnet',
  nativeCurrency: { name: 'Matic', symbol: 'MATIC', decimals: 18 },
  rpcUrls: ['https://rpc-mumbai.matic.today'],
  blockExplorerUrls: ['https://mumbai.polygonscan.com/'],
};
```
Note that `chainId` must be given as a **hexadecimal string**, not in decimals. You can find the network details [here](https://docs.polygon.technology/docs/develop/network-details/network).

### Connect with Metamask
Next, we will define a function to connect with Metamask and get the `Proivder` instance. You can notify the user with a modal to switch the network by calling `wallet_addEthereumChain`. If the network does not exist, calling this method will add the network to the Metamask's network list.
```Typescript
// src/context/WalletProvider.tsx
import { ethers } from 'ethers';

async function connectWalletMetamask() {
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

## Transfer NFTs
To transfer NFTs, you need to:
1. create transaction data.
2. sign the data with Metamask.
3. post the data with signature to [`transfer`](../../reference/swagger-v2.yaml#transfer) endpoint.

The request body should include the following information:
- `from`: sender address
- `to`: contract address
- `value`: amount of MATIC to send (=0)
- `gas`: gas
- `nonce`： nonce of the sender
- `data`： data which will be given to the contract to execute transfer
- `signature`: signature generated with Metamask
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

### 1. Create trasaction data
Transaction data looks like:
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

Firstly, prepare and encode the transaction data with the [ABI](https://github.com/0xhokusai/hokusai-api-client-sample/blob/main/src/abis/ERC721WithRoyaltyMetaTx.json).

Solidity's Document has an explanation of the ABI, so understanding it will help you visualize the implementation.
What is [ABI?](https://solidity-jp.readthedocs.io/ja/latest/abi-spec.html)

```typescript
// src/component/TransferForm.tsx
import { ethers } from 'ethers';
// https://github.com/0xhokusai/hokusai-api-client-sample/blob/main/src/abis/ERC721WithRoyaltyMetaTx.json
import HokusaiAbi from '../abis/ERC721WithRoyaltyMetaTx.json';

// get the wallet address
const signer = provider.getSigner();
const from = await signer.getAddress();

// load ABI
const hokusaiInterface = new ethers.utils.Interface(HokusaiAbi.abi);

// generate encoded data
const data = hokusaiInterface.encodeFunctionData('transferFrom', [
   from,
   toAddress,
   tokenId,
]);
```
Arguments for `encodeFunctionData` represent:
- `from`: wallet address to transfer an NFT from
- `toAddress`: wallet address to tranfer an NFT to
- `tokenId`: ID of the NFT to transfer


Then, set up other parameters.
```typescript
// src/component/TransferForm.tsx
import ForwarderAbi from '../abis/MinimalForwarder.json';

type Message = {
  from: string;
  to: string;
  value: number;
  gas: number;
  nonce: number;
  data: string;
};


// configure forwarder contract
const forwarder = new ethers.Contract(
  forwarderAddress, // 0x0E285b682EAF6244a2AD3b1D25cFe61BF6A41fc3 <- for testnet
  ForwarderAbi.abi,
  provider
);

const message: Message = {
  from,
  to: contractAddress, // 0x73b5373a27f4a271c6559c6c83b10620acde9a2a <- for testnet
  value: 0,
  gas: 1e6,
  nonce: (await forwarder.getNonce(from)).toNumber(),
  data,
};

```
`forwarderAddress` and `contractAddress` should be set as follows:

address | Mumbai Testnet | Polygon Mainnet 
--------|----------------|----------------
`forwarderAddress` | 0x0E285b682EAF6244a2AD3b1D25cFe61BF6A41fc3 | 0xD64a425d91a97866cE4ee2d759A23560411ADb01
`contractAddress` | 0x73b5373a27f4a271c6559c6c83b10620acde9a2a | Your contract address

```
forwarderAddress: 0x0E285b682EAF6244a2AD3b1D25cFe61BF6A41fc3 // For Mumbai
contractAddress: 0x73b5373a27f4a271c6559c6c83b10620acde9a2a // For Mumbai
```

`forwarder.getNonce(from)` returns the nonce which you need for the transaction to be mined. 


### 2. Sign the data with Metamask
To sign the data, you need to:
1. create signed data (with `message`)
2. show a Metamask modal to request the user to allow signing


#### Create signed data
Create [`signTypedData_v4`](https://docs.metamask.io/guide/signing-data.html#sign-typed-data-v4
).

Define a fucntion `createTypedDataV4()` as follows.
```typescript
// src/utils/TypedData.ts
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

Then, call `createTypedDataV4()` to create signed data.
```typescript
// src/component/TransferForm.tsx
const { chainId } = await provider.getNetwork();

const typedData = createTypedDataV4(
  chainId,
  forwarderAddress, // 0x0E285b682EAF6244a2AD3b1D25cFe61BF6A41fc3
  message
);
```
#### Show Metamask modal to request user to allow signing

Signature data is stored as `signature`.
```typescript
// src/component/TransferForm.tsx
const signature = await provider.send('eth_signTypedData_v4', [
  from,
  JSON.stringify(typedData),
]);
```


### 3. Post!
Finally, post `message` and `signature` to [`transfer`](../../reference/swagger-v2.yaml#transfer) endpoint. 
Set your own `contractId` and `apiKey`.

<!--
type: tab
title: v2
-->

```typescript
// src/component/TransferForm.tsx
const contractVer = 'your-contract-version'
const contractId = 'your-contract-id'
const apiKey = 'your-api-key'

result = await fetch(
  `https://mumbai.hokusai.app/v2/nft/${contractVer}/${contractId}/transfer?key=${apiKey}`,
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
// src/component/TransferForm.tsx
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

## Summary
To perform transfer of NFTs with no gas payment using Hokusai, you need to:

1. create transaction data.
2. sign the data with Metamask.
3. post the data with signature to [`transfer`](../../reference/swagger-v2.yaml#transfer) endpoint.
