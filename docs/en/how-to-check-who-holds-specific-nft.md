# How to check who holds specific NFT by using Hokusai API

## Introduction

Today, we will introduce the following implementation example using the Hokusai API.

- Verify that the user who connected the wallet has a specific NFT.
- Verify how many NFTs the user connected to the wallet has.

## What is Hokusai API?

[Hokusai API](https://hokusai.app/) is a service that provides APIs for NFT.
You can these operation by using Hokusai API.

- Mint NFT
- Transfer NFT
- Burn NFT
- Set Royalty
- Get Metadata information

You can create a service that utilize NFT without writing the contract yourself.

## Issue API Key

We are providing API keys for Testnet free.
You can apply to use the service by filling out the form [here](https://ir9l8pcvcmm.typeform.com/to/xSbuj2WA).

## About wallet

we use Metamask, which is a common wallet.

- [About metamask](https://docs.metamask.io/guide/#why-metamask)

the below code is example for connecting Metamask.

```typescript
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

## Load the ABI of contract

To call functions for the contract, let's load the ABI.

The ABI used by Hokusai API is [here](https://github.com/0xhokusai/hokusai-api-client-sample/blob/main/src/abis/ERC721WithRoyaltyMetaTx.json).

You can find `contractAddress` in the email sent when you applied for API or in the [administration page](https://dashboard.hokusai.app).

```Typescript
import HokusaiAbi from '../abis/ERC721WithRoyaltyMetaTx.json';

const contractAddress = 'Your ContractAddress'
const hokusaiContract = new ethers.Contract(
  contractAddress,
  HokusaiAbi.abi,
  connectMetamask()
);
```

## Find NFT owner

You can find an address that hold specific NFT by checking to equal the address of NFT owner and wallet address.

```typescript
const signer = provider.getSigner(0); // getting Metamask wallet
const address = await signer.getAddress(); // getting wallet address
const tokenId = 1;
const tokenHolder = await contract.ownerOf(tokenId); // get the owner of NFT specified by {tokenId}

const isHolder = tokenHolder === address; // NFT所有者とMetamaskのアカウントを比較
```

## How much NFT does the user that connect wallet hold?

Contracts issued by Hokusai API are compatible ERC721Enumerable.

By using this specification, you can implement website that only users that hold NFT minted by the contract can access.

```typescript
const signer = provider.getSigner(0);
const address = await signer.getAddress();
const tokenBalance = await contract.balanceOf(address);
```

If `tokenBalance > 0`, you can check that the user hold NFT.

## Conclusion

Today, we introduced below examples.

- By utilizing `ownerOf(tokenId)` , check whether the user that connect wallet hold specific NFT.
- By utilizing `balanceOf(address)`, check how many NFTs the user that connect wallet have.

These can be used to implement pages that can only be accessed by users who own the NFTs minted in that contract.

## Advertisement

Hokusai provides API that is development infrastructure for NFT.

It is an embedded NFT service that provides API services as an infrastructure for all individuals and businesses that want to distribute value digitally.

https://hokusai.app/

Japan MonoBundle, the provider of "Hokusai API", is hiring engineers!
Why don't you join us as a full remote engineer and enjoy the speed and big changes?

[Hokusai career page](https://www.notion.so/0xhokusai/Backend-engineer-aabdbbbb48584113854e9e8102f13d6b)

## Who wrote this article?

<img src="https://storage.googleapis.com/zenn-user-upload/e9d420e447090c39b0d22af2.png" width=250>

**Tarumi - CTO**

[https://y.at/🔥🍞🔥🍞🔥](https://y.at/%F0%9F%94%A5%F0%9F%8D%9E%F0%9F%94%A5%F0%9F%8D%9E%F0%9F%94%A5)
