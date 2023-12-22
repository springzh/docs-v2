# Tables and Chains

## Supported Chains

ZettaBlock supports the following chains and is continuously onboarding new chains.

| Blockchain            | Data Lake | Real Time | EVM Compatible |
|-----------------------|-----------|-----------|----------------|
| Arbitrum              | Yes       | No        | Yes            |
| Avalanche C Chain     | Yes       | Yes       | Yes            |
| Avalanche P Chain     | Yes       | Yes       | No             |
| Base                  | Yes       | Yes       | Yes            |
| Beacon Goerli         | No        | Yes       | No             |
| Beacon Mainnet        | No        | Yes       | No             |
| Bitcoin               | Yes       | No        | No             |
| BSC                   | Yes       | No        | Yes            |
| Ethereum Goerli       | Yes       | Yes       | Yes            |
| Ethereum Mainnet      | Yes       | Yes       | Yes            |
| Polygon Mainnet       | Yes       | No        | Yes            |
| Polygon Mumbai        | Yes       | No        | Yes            |
| Polygon zkEVM         | Yes       | No        | Yes            |
| Ripple                | Yes       | No        | No             |
| Solana                | Yes       | No        | No             |
| Sui                   | Yes       | No        | No             |
| zkSync                | Yes       | Yes       | Yes            |

## Table Typees

There are several types of tables:

**Decoded Raw Tables**: Row tables with decoded data saved together. Includes blocks, transactions, logs and traces for EVM chains. There are different tables for various Non-EVM chains.
**Core Event Tables**: Core event tables derived from logs, includes erc20_evt_transfer, erc721_evt_transfer, erc1155_evt_transfer_batch, erc1155_evt_transfer_single, eth_transfers etc.
**Metadata Tables**: Includes tables derived from logs: contract_creations, erc20_tokens, nft_tokens. And tables integrated from offchain data sources: token prices, NFT collection metadata and NFT token metadata tables.
**Balance Tables**: Real time token balance tables: native_balances, fungible_token_balances.
**Abstraction Tables**: Tables built on top of above table types for specific category or protocol. These tables are stored in various databases to better reflect their usage. Most popular databases for abstraction table include: dex, nft, ens, bridges, lending, staking, etc.




Here is an overview the tables available:

Raw/Decoded data - blocks, transactions, logs and traces
Raw and decoded data are merged and supported under the raw data tags (see above) as of 2022/12/02
Metadata Tables - erc20, nft_tokens, includes names, ids, symbols, decimals
Offchain Abstraction data - token prices
Core event data - erc721.ERC721_evt_Transfer, erc20.ERC20_evt_Transfer, erc1155.ERC1155_evt_TransferSingle
dApp specific data - such as opensea.WyvernExchange_evt_OrdersMatched, opensea.WyvernExchange_call_atomicMatch_
DEX - pools, swaps, liquidity actions
NFT - mints, trades overall and by marketplace
ENS - ENS names, Twitter handles, owners, and more, for each ENS Registration and Renewal event
Bridges - bridge name, chain_id and inflow (bool) for Ethereum



Decoded Base Tables
i. EVM
ii. Non-EVM
Abstraction Tables
i. DeFi
ii. NFT
iii. Bridges
iv. ...
