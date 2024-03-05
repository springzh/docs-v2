# Data Catalog

ZettaBlock provides a wide range of blockchain tables across EVM and non-EVM ecosystems for both indexing and analytics purposes. 
For detailed information on ZettaBlock's data models, please visit our [Data Catalog](https://docs.zettablock.com/page/data-catalog).

## Tables Supported

Here is an overview the tables available on ZettaBlock at different levels of abstraction.

| Data Type       | Description                                                                                    |
|:----------------|:-----------------------------------------------------------------------------------------------|
| Raw             | Blocks, transactions, logs and traces tables, with decoded info saved in same table.           |
| Decoded         | ERC20, nft_tokens and other decoded event tables.                                              |
| Transfers       | ERC20, ERC721 and ERC1155 transfer data.                                                       |
| Balances        | Wallet's ERC20, ERC721 and ERC1155 tokens latest balance or real time balance data.            |
| DEX             | DEX protocol related abstraction tables, such as pools, swaps tables.                          |
| NFT             | NFT related abstraction tables.                                                                |
| Misc            | Other abstraction tables such as ENS and labels.                                               |

# Chains Supported

We support the following chains in data lake and realtime (with ![realtime_icon](https://app.zettablock.com/assets/icons/toggle.svg)) across multiple data types. More chains and types are being onboarded and will coming soon.

We support the following chains in data lake and realtime. More chains are being onboarded and will coming soon.

| Type        | Chain                 | Raw &nbsp; &nbsp; &nbsp; &nbsp; | Decoded &nbsp; &nbsp; | Transfers &nbsp;| Balances &nbsp; | DEX &nbsp; &nbsp; &nbsp; &nbsp; | NFT &nbsp; &nbsp; &nbsp; &nbsp; | Misc          |
|:------------|:----------------------|:--------------|:--------------|:--------------|:--------------|:--------------|:--------------|:--------------|
| EVM         | Ethereum              | ✅ ![realtime_icon](https://app.zettablock.com/assets/icons/toggle.svg)  | ✅ ![realtime_icon](https://app.zettablock.com/assets/icons/toggle.svg)  | ✅ ![realtime_icon](https://app.zettablock.com/assets/icons/toggle.svg)  | ✅ ![realtime_icon](https://app.zettablock.com/assets/icons/toggle.svg)  | ✅            | ✅             | ✅            |
|             | Polygon               | ✅            | ✅             | ✅            |               | ✅            | ✅             |               | 
|             | Base                  | ✅            | ✅             | ✅            |               | ✅            |               |               | 
|             | zkSync                | ✅            | ✅             | ✅            |               | ✅            |               |               | 
|             | Polygon zkEVM         | ✅            | ✅             | ✅            |               |               |               |               |
|             | BSC                   | ✅            | ✅             | ✅            |               |               |               |               |
|             | Arbitrum              | ✅            | ✅             | ✅            |               |               |               |               |
| Non-EVM     | Solana                | ✅            | ✅             | ✅            | ✅            |               |                | ✅            | 
|             | Sui                   | ✅ ![realtime_icon](https://app.zettablock.com/assets/icons/toggle.svg)  | ✅             |               |               | ✅            | ✅             |              | 
|             | Beacon                | ✅ ![realtime_icon](https://app.zettablock.com/assets/icons/toggle.svg) |               |               |               |               |               |               |
|             | Bitcoin (coming soon) |               |               |               |               |               |               |               |

## Data Status and Freshness

For detailed table status and freshness in each chain, please check out the Status page (coming soon).

## Appendix

For those interested, the diagram below showcases **a high-level overview of ZettaBlock's approach to creating tables from raw blockchain data**. 

As shown below, the hierarchy is split into 3 levels of tables - bronze, silver and gold. Bronze tables are derived from raw on-chain data that is then combined with decoded on-chain data and funneled to create the silver layer of tables. Through further data modeling, the abstraction or gold level of tables will be eventually created.

![image](https://files.readme.io/729ae3d-schema-Top_Level_View.drawio.png)
