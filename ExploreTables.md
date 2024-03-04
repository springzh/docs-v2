# ZettaBlock tables

In this section, you can find descriptions of all ZettaBlock tables and supported chains at different levels of abstraction.

Here is an overview the tables available:

**Raw** - blocks, transactions, logs and traces tables, with decoded info saved in same table.

**Decoded** - ERC20, nft_tokens and other decoded event tables.

**Transfers** - ERC20, ERC721 and ERC1155 transfer data.

**Balances** - Wallet's ERC20, ERC721 and ERC1155 tokens latest balance or real time balance data.

**DEX** - DEX protocol related abstraction tables, such as pools, swaps tables.

**NFT** - NFT related abstraction tables.

**Misc** - Other abstraction tables.

# Available Chains

We support the following chains in data lake and realtime. More chains are being onboarded and will coming soon.

| Chain                 | Raw           | Decoded       | Transfers     | Balances      | DEX           | NFT           | Misc          |
|-----------------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|
| Ethereum              | ✅(realtime)  | ✅(realtime)   | ✅(realtime)  | ✅(realtime)  | ✅            | ✅             | ✅            |
| Polygon               | ✅            | ✅             | ✅            |               | ✅            | ✅             |               | 
| Bitcoin               | ✅            |               |               |               |               |               |               |
| Base                  | ✅            | ✅             | ✅            |               | ✅            |               |               | 
| Solana                | ✅            | ✅             | ✅            | ✅            |               |                | ✅            | 
| Sui                   | ✅(realtime)  | ✅             |               |               | ✅            | ✅             |              | 
| zkSync                | ✅            | ✅             | ✅            |               | ✅            |               |               | 
| Polygon zkEVM         | ✅            | ✅             | ✅            |               |               |               |               |
| BSC                   | ✅            | ✅             | ✅            |               |               |               |               |
| Arbitrum              | ✅            | ✅             | ✅            |               |               |               |               |
| Beacon                | ✅(realtime)  |               |               |               |               |               |               |

# Table Availability, Status And Freshness

For detailed tables available in each chain, please check our [Data Catalog](https://stage-app.zettablock.dev/v2/explore/tables) page. 

Please refer to the [Status](https://stage-app.zettablock.dev/v2/freshness) page for table status and freshness.
