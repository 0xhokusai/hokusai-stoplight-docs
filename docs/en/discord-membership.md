# Create NFT memberships for your Discord

### Create NFTs which represent membership to a specific discord server or channel.

A few examples:

- Large NFT collections which grant every holder access to a private community (ie. [Bored Ape Yacht Club](https://opensea.io/collection/boredapeyachtclub))
- Smaller NFT collections which grant holders lifetime access to an existing paid community ([Shiny Magpies](https://opensea.io/collection/shiny-magpies))

## Set up NFT

More detailed steps to mint an NFT with Hokusai are explained in the [tutorial](get-started.md).

### 1. Obtain API Key

Submit your request for a Hokusai API Key [here](https://0xhokusai.notion.site/Hokusai-API-Application-form-a6d8118d416b41d88632396e3156cddb) and receive an email, which contains `HOKUSAI_API_KEY` and `HOKUSAI_CONTRACT_ID`.

### 2. Obtain nft.storage API Key

Register an [nft.storage](http://nft.storage) account [here](https://nft.storage/login/). Create an API Key at **API Keys** > **+ New Key**.


### 3. Clone sample project

To mint an NFT with Hokusai, clone this sample repository.

```bash
$ git clone https://github.com/0xhokusai/hokusai-get-started.git
```

### 4. Create a setting file

Run the code below to copy `.env.sample` file from the project and create a `.env` file.

```bash
$ cp .env.sample .env
```

Edit `.env` and set the variables. Note you **don't** need to set the `WALLET_PRIVATE_KEY` to perform mint operations.

```bash
# Leave blank.
WALLET_PRIVATE_KEY = ""
# Your nft.storage API Key.
NFT_STORAGE_API_KEY = "your-nft-storage-api-key"
# Contract Network
CONTRACT_NETWORK = "the-network-your-deployed-contract"
# Your Hokusai API Key.
HOKUSAI_API_KEY = "your-hokusai-api-key"
# Your Hokusai Contract ID.
HOKUSAI_CONTRACT_ID = "your-contract-id"
# Your Hokusai Contract Address.
HOKUSAI_CONTRACT_ADDRESS = "your-contract-address"
```

### 5. Install packages

Run the code below to install all the packages required to execute programs.

```bash
$ yarn
```

### 6. Publish NFT metadata

You can publish metadata using our sample image (hokusai.png) by running the following command. You can edit the metadata to upload in `src/storeMetadata.ts`.

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

### 7. Mint an NFT

To [mint](glosarry.md#mint) an NFT, run the code below.
```bash
$ yarn mint-nft {to} {tokenUri}
{
  txHash: '0x8765feaa11a7e0f9f4a84f21415434d80dd9be27728a8f6eff4d402e4d0c2766' # example Transaction Hash
}
```
Refer to our documentation [here](../../reference/swagger-v2.yaml#mint-a-new-nft) for parameter descriptions.

## Set up roles on Discord

We'll be using the [collab.land](https://collab.land/) Discord bot. More details can be found [here](https://collabland.freshdesk.com/support/solutions/articles/70000036689-discord-bot-walkthrough).

1. Invite the bot into your server using this [Invite Link](https://discord.com/oauth2/authorize?client_id=704521096837464076&scope=bot&permissions=8).
2. In the **#collabland-config** channel, send a message `!setup role` to set up a role based on NFT ownership.
3. Choose `MATIC`.
4. Select 🤑 for NFT (ERC721).
5. Input the details of the NFT you'd like to grant access to the channel.
    1. `contract address` can be found in your [dashboard](https://dashboard.hokusai.app/) under `Contract` page. It looks like **[0x8Cf0...5ac9]**.
    2. `token id` can be found in [Polygonscan](https://polygonscan.com/) with a `txHash` or `contract address`. More details can be found [here](get-started.md#11-check-tokenid-on-polygonscan).


## 🎊 That's all! NFT holders can now access private channels

When your community member joins the discord, they need to send `!join` to get set up with collab.land. After setting up with the bot, they will be granted roles which unlock access to the chosen channels based off the chosen NFTs in their wallet.

