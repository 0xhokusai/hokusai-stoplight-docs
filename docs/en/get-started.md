# Getting Started with Hokusai 🌊
> This document covers the setup and basic usages of Hokusai. <br>
> After completing this tutorial,  you'll be able to use Hokusai and start integrating NFT on your website. 
> 
### Table of Contents
**[Getting Started](#get-started)**<br>
**[Using Hokusai](#use-hokusai-api)**<br>

## Get Started
To get started with Hokusai, first, clone this repository and follow the below tutorial.
```:bash
git clone https://github.com/0xhokusai/hokusai-get-started.git
```
The installation requires the following steps:
- Obtain API key
- Create your wallet
- Publish NFT metadata
- Mint NFT

### 1. Obtain your API key
Submit your request for an API key [here](https://0xhokusai.notion.site/Hokusai-API-Application-form-a6d8118d416b41d88632396e3156cddb). You will receive an email, which contains `HOKUSAI_API_KEY` and `HOKUSAI_CONTRACT_ID`. Currently, it takes 2-3 business days to issue the API Key if you sign up for a Mainnet key. A Testnet key will be issued instatnly after your request.

### 2. Prepare a wallet

Before you start dealing with NFTs, you need an "account" to hold your tokens. Wallet applications are usually used to create an account on a blockchain as well as to sign your transactions. In this section, we will explain how to set up Metamask, which is one of the most common wallet applications for Ethereum and other networks, to create your wallet and your account, and connect them to the Polygon network. If you already have a wallet, proceed to [3. Obtain an nft.storage API Key](./get-started.md#3-obtain-an-nftstorage-api-key).

#### 2.1. Install Metamask

Install Metamask [here](https://metamask.io/download.html). Metamask supports Chrome, Firefox, Brave, and Edge.

#### 2.2. Create a wallet

1. Open Metamask on your browser.
2. Press **Get Started.**
3. Select **Create a Wallet** on the right.    

    ![create_a_wallet.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/M9kJf3upBS8)
    
4. Read the data usage policy and privacy policy, then select either button.
5. Create a new password, read and agree to the terms of use, and press **Create**.
6. Watch the video clip, and press **Next**.
7. Open up your Secret Recovery Phrase, copy the phrase somewhere, then press **Next**. (**Never** share your Secret Recovery Phrase with anyone and store it somewhere safe.)
8. Confirm your Secret Recovery Phrase by selecting the words in the right order, and press **Confirm**.
9. Press **All Done**.

Now your wallet has been created along with your account.

#### 2.3. Connect to Polygon

At this point, the account you've just created does not exist on the Polygon network yet. Now, we will be adding the Polygon network and connecting your account to the network.

1. Press **Ethereum Mainnet** on the top right side of the page.
2. Press **Custom RPC** at the bottom of the menu.
    
    ![custom_rpc.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/wVrrFHw5p3M)
    
3. Fill out the fields with information below. For `New RPC URL`, choose one of the RPCs on the table and use another if it fails to connect to the network.　This information can be found on the [network(chain)](network.md#rpc-infomations) page. This time you will use `polygon-mumbai` information.
    
    ![new_network_mainnet.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/ZmYEkZY00fM)
    
4. Press **Save**.

Confirm that the currency of your balance is **MATIC** and the button on the top right side of the page says either **Polygon Mainnet** or **Mumbai Testnet**, and now you have successfully connected to the network.

### 3. Obtain an nft.storage API Key

NFTs often have a metadata URI as one of their properties. This project makes use of [nft.storage](http://nft.storage) to store metadata for NFTs we will be minting. [nft.storage](https://nft.storage/) is a decentralized storage system to store and publish NFT metadata on [IPFS](https://docs.ipfs.io/).

To use [nft.storage](http://nft.storage), you need to register and create an API Key.

1. Register an [nft.storage](http://nft.storage) account [here](https://nft.storage/login/).
2. After you signed in, press **API Keys** on the right top.
3. Press **+ New Key** on the right top.
4. Name an API Key and press **Create**.

Now you have created an [nft.storage](http://nft.storage) API Key.

### 4. Set up the project

#### 4.1. Create a setting file

In this project, the program reads Hokusai API Key, Hokusai Contract ID, [nft.storage](http://nft.storage) API Key and the wallet's private key into environment variables, from a `.env` file. Run the code below to copy `.env.sample` file from the project and create a `.env` file.

```bash
$ cp .env.sample .env
```

Edit `.env` and set the variables. Note you don't need to set the `WALLET_PRIVATE_KEY` here yet.

```bash
#Your wallet's private key. Refer to "Transfer an NFT" for details.
WALLET_PRIVATE_KEY = "your-private-key"
#API key for nft.storage
NFT_STORAGE_API_KEY = "your-nft-storage-api-key"
#Contract Network (please set "polygon-mumbai" in this time)
CONTRACT_NETWORK = "polygon-mumbai"
#Hokusai API key
HOKUSAI_API_KEY = "your-hokusai-api-key"
#Hokusai Contract Version
HOKUSAI_CONTRACT_VERSION = "your-contract-version"
#Hokusai contract ID
HOKUSAI_CONTRACT_ID = "your-contract-id"
#Hokusai Contract Address
HOKUSAI_CONTRACT_ADDRESS = "your-contract-address"
```

#### 4.2. Install packages

Run the code below to install all the packages required to execute programs.

```bash
$ yarn
```

### 5. Publish NFT metadata

You can publish metadata using our sample image (hokusai.png) by running the following command.

```bash
$ yarn store-metadata
```

You will get an URL for the metadata uploaded on IPFS, which consists of the name, description and image. You can access the metadata by the URL via HTTPS requests.

```bash
$ curl https://dweb.link/ipfs/bafyreieaaqfof34kfqyvwe4arta6jsuwuauim4d24qo22ct2xnvjnlnrb4/metadata.json

{
    "name":"nft.storage store test",
    "description":"Using the nft.storage metadata API to create ERC-1155 compatible metadata.",
    "image":"ipfs://bafybeicsu73gednfaa5svozuoac4ebpi76nn4auhygcvkvbn4kk2vdv5ey/hokusai.png"
}
```

## Use Hokusai
Congratulations! You're ready to use Hokusai. Now, let's try minting and getting an NFT. 
If you using the Mumbai testnet, change the baseUrl of src/getNft.ts and src/mintNft.ts's to https://mumbai.hokusai.app.

### 1. Mint an NFT
To [mint](glosarry.md#mint) an NFT, run the code below
```bash
$ yarn mint-nft {to} {tokenUri}
{
  txHash: '0x8765feaa11a7e0f9f4a84f21415434d80dd9be27728a8f6eff4d402e4d0c2766' # example Transaction Hash
}
```

In the tokenURI field, paste the URL that was issued when you published the NFT metadata in advance.

Refer to our documentation [here](../../reference/swagger-v2.yaml#mint-a-new-nft) for parameter descriptions.

Hokusai v2 is the default for all command in get-started, including `mint-nft`.
If you want to use Hokusai v1 API, use `:v1` suffix.

```bash
# call Hokusai v1 example
$ yarn mint-nft:v1 {to} {tokenUri}
```

#### 1.1. Check TokenID on Polygonscan
You have successfully minted an NFT, but where can you actually see this token on the chain? You can confirm this by scanning the transaction with Polygonscan, which is a website where you can explore all the transactions/blocks/accounts on a Polygon chain (in this case, Mumbai Testnet) with a hash/block number/address.

Mumbai Testnet： https://mumbai.polygonscan.com

Polygon Mainnet： https://polygonscan.com


![polygonscan_top.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/T7773jY2LH8)

Copy & paste in the search box the `txHash` you received as the response and enter.

![polygonscan_detail.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/9tB4KnKIDrA)

Now all the details about this transaction (minting), including the block, contract it intereacted with, information about tokens transferred, etc, show up. Look for the **Tokens Transferred** > **For ERC-721 TokenID [`tokenId`]**. The number in `[]` is the `tokenId` of the token (In the picture above, `tokenId` is `66`). You will use this `tokenId` with the following operations.


### 2. Get an NFT
`tokenId` issued by Hokusai can be viewed via [polygonscan](https://mumbai.polygonscan.com). You can search by txHash received from minting. 

```bash
$ yarn get-nft {tokenId}
{ id: {tokenId}, tokenUri: 'https://example.com/1' } # example response
```

### 3. Transfer an NFT

To transfer an NFT, you need a signature of the sender account. Generating the signature requires the sender's private key. 

#### 3.1. Export Private Key

We will explain how to export your private key on Metamask.

1. Open Metamask on your browser.
2. Press the menu button just below your account icon on the top right side of the page.
3. Press **Account details**.
    
    ![account_details.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/DyqaoXNXV08)
    
4. Press **Export Private Key**.
5. Type your password and press **Confirm**.
6. Copy the private key and press **Done**.

Set your private key to `WALLET_PRIVATE_KEY` in `.env`.

```bash
#Your wallet's private key. Refer to "Transfer an NFT" for details.
WALLET_PRIVATE_KEY = "your-private-key"
#API key for nft.storage
NFT_STORAGE_API_KEY = "your-nft-storage-api-key"
#Contract Network (please set "polygon-mumbai" in this time)
CONTRACT_NETWORK = "polygon-mumbai"
#Hokusai API key
HOKUSAI_API_KEY = "your-hokusai-api-key"
#Hokusai Contract Version
HOKUSAI_CONTRACT_VERSION = "your-contract-version"
#Hokusai contract ID
HOKUSAI_CONTRACT_ID = "your-contract-id"
#Hokusai Contract Address
HOKUSAI_CONTRACT_ADDRESS = "your-contract-address"
```

> If the wallet service is supported, there is no need to manage the private key of the sender's wallet.

>  Your private key is very sensitive information. **Do not** share with anyone else.


#### 3.2. Transfer
Run the code below. `{to}` should be the account address you want to send the token to.

```bash
$ yarn transfer-nft {to} {tokenId}
{
  txHash: '0xdec77ee7148dc796dd08d656a256e1466daf2763c08cfe104f76e8baf318f3ed' # example Transaction Hash
}
```

#### 3.3. Check transaction on Polygonscan
You can view this transfer transaction on [Polygonscan](https://mumbai.polygonscan.com/) with `txHash`. Copy & paste the `txHash` in the search box.

![polygonscan_detail_transfer.png](https://stoplight.io/api/v1/projects/cHJqOjg0NjEy/images/lOIe8DV0J5E)

In **Tokens Transferred** section, you can see from which address and to which address the token of `[tokenId]` was transferred. Confirm the wallet address and `{to}` correspond `From` and `To` on the page. 


### 4. Burn an NFT

Run the code below. 
Must be the owner of the NFT for that `tokenId` to burn. 

1. Fill in the private key of your wallet in the `.env` file ([Get the private key](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key)) 

2. Run the code below


```bash
$ yarn burn-nft {tokenId}
{
  txHash: '0x67eca6ca63d542f4b01fd60d53feda89ce64f42394c27b77fa3fccbb15244d3c' # example Transaction Hash
}
```

So far, you have minted an NFT, got NFT info, and transferred an NFT via Hokusai 🥳
