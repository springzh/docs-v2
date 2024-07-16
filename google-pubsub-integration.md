# ZettaBlock Web3 Data Google Pub/Sub Service Integration

ZettaBlock is integrating with Google Pub/Sub service for its real-time Web3 blockchain data. Integration for Sui blockchain has been implemented. You can find more details about available topics to subscribe in this document. More chains such as Bitcoin, Ethereum will coming soon. 

You can easily consume our real-time data with simple steps:
- Setup a subscription to a topic
- Listen to the subscription to receive new data

Below is introduction to get started on the available topics, the attributes for each topic, and some sample filters on those attributes.

# Quick Start

All ZettaBlock Pub/Sub topics are provided in project `zettablock-data` project. You can quickly setup a subscrition to one of the available topics and start receiving data.

## Setup a new subscription

You can follow the [Create pull subscriptions ](https://cloud.google.com/pubsub/docs/create-subscription) to create a new subscription. 

Here are list of full paths to our Sui mainnet PubSub topics:
- projects/zettablock-data/topics/sui_mainnet_balance_changes_raw
- projects/zettablock-data/topics/sui_mainnet_checkpoints_raw
- projects/zettablock-data/topics/sui_mainnet_events_raw
- projects/zettablock-data/topics/sui_mainnet_object_changes_raw
- projects/zettablock-data/topics/sui_mainnet_transaction_blocks_raw
- projects/zettablock-data/topics/sui_mainnet_transactions_raw

## Define a filter (Optional)

Depending on the topic you select to subscribe, there are various attributes you can choose to apply a filter for the messages. With a filter applied, you'll only receive messages from the subscription when the filter condition match. 

For example, to monitor all events from a certain package of Sui blockchain, you can use the following filter on the `sui_mainnet_events_raw` topic:

```
attributes.package_id = "0x8d97f1cd6ac663735be08d1d2b6d02a159e711586461306ce60a2b7a6a565a9e"
```

## Receive message from subscription

Suppose you created a subscription to a topic and named it as "event_by_package_id_sub". You can receive data from the subscription now. Below is a sample code snippet in Python:

```python
from google.cloud import pubsub_v1
from concurrent.futures import TimeoutError
import io
import json
from google.cloud.pubsub import SubscriberClient
from google.oauth2 import service_account

project_id = "zettablock-data"
subscription_id = "event_by_package_id_sub"
timeout = 120.0

subscriber = SubscriberClient()
subscription_path = subscriber.subscription_path(project_id, subscription_id)

def callback(message: pubsub_v1.subscriber.message.Message) -> None:
    encoding = message.attributes.get("googclient_schemaencoding")
    if encoding == "JSON":
        message_data = json.loads(message.data)
        print(f"Received a JSON-encoded message:{message_data}")
    else:
        print(f"Received a message with no encoding:\n{message}")
    message.ack()

streaming_pull_future = subscriber.subscribe(subscription_path, callback=callback)
print(f"Listening for messages on {subscription_path}..\n")

with subscriber:
    try:
        streaming_pull_future.result(timeout=timeout)
    except TimeoutError:
        streaming_pull_future.cancel()
        streaming_pull_future.result()
        print(f"No data after {timeout}..\n")

```

# Sui Mainnet Topic Details

## Topic projects/zettablock-data/topics/sui_mainnet_balance_changes_raw

Schema:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| checkpoint_digest            | String                | Checkpoint sequence digest                                                       |
| block_time                   | String                | Block time                                                                       |
| index                        | Int                   | Index in the checkpoint                                                          |
| owner_type                   | String                | Owner type                                                                       |
| owner_address                | String                | Owner address                                                                    |
| initial_shared_version       | Long                  | Initial shared version                                                           |
| coin_type                    | String                | Coin type                                                                        |
| amount                       | Double                | Balance change amount                                                            |
| process_time                 | String                | Process time                                                                     |
| block_date                   | String                | Block date                                                                       |


Attributes:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| owner_type                   | String                | Owner type                                                                       |
| owner_address                | String                | Owner address                                                                    |
| coin_type                    | String                | Coin type                                                                        |
| block_date                   | String                | Block date                                                                       |
| index                        | Int                   | Index in the checkpoint                                                          |

Sample message:
```json
{
    "transaction_block_digest": "GdsbVCk9655YeLsAisSefP2KTadzworJSthcrynBmnaq",
    "checkpoint_sequence_number": 88573240,
    "checkpoint_digest": "AcgSPLvs1dqsnMQXYqEKuuDwCahDbxBtZfiviSE84qEf",
    "block_time": "2024-07-14T20:10:56+00:00",
    "index": 0,
    "owner_type": "AddressOwner",
    "owner_address": "0x33a962ee7746b8be68147f699eb73b2725efdb93c0b2015267f5566677b1e742",
    "initial_shared_version": 0,
    "coin_type": "0x2::sui::SUI",
    "amount": -1024624,
    "process_time": "2024-07-15T06:15:13+00:00",
    "block_date": "2024-07-14"
} 
```

## Topic projects/zettablock-data/topics/sui_mainnet_checkpoints_raw

Schema:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| sequence_number              | Long                  | Checkpoint block digest                                                          |
| digest                       | String                | Checkpoint sequence digest                                                       |
| previous_digest              | String                | Previous Checkpoint sequence digest                                              |
| epoch                        | Int                   | Epoch number                                                                     |
| total_gas_cost               | Long                  | Total gas cost                                                                   |
| total_computation_cost       | Long                  | Total computation cost                                                           |
| total_storage_cost           | Long                  | Total storage cost                                                               |
| total_storage_rebate         | Long                  | Total storage rebate                                                             |
| non_refundable_storage_fee   | Long                  | Non-refundable storage fee                                                       |
| num_of_transaction_blocks    | Int                   | Number of transaction blocks in this checkpoint                                  |
| network_total_transactions   | Int                   | Network total transactions count                                                 |
| timestamp                    | String                | Checkpoint timestamp                                                             |
| checkpoint_commitments       | String                | Checkpoin commitments                                                            |
| validator_signature          | String                | Validator signature                                                              |
| end_of_epoch                 | Bollean               | Whether or not the end of current epoch                                          |
| end_of_epoch_data            | String                | Data for end of current epoch                                                    |
| process_time                 | String                | Process time                                                                     |
| block_date                   | String                | Block date                                                                       |


Attributes:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| sequence_number              | Long                  | Checkpoint block digest                                                          |
| digest                       | String                | Checkpoint sequence digest                                                       |
| epoch                        | Int                   | Epoch number                                                                     |
| validator_signature          | String                | Validator signature                                                              |
| block_date                   | String                | Block date                                                                       |


Sample message:
```json
{
    "sequence_number": 83143254,
    "digest": "HNS3XSfR7szk7GZt3rK3BxZTBAQbqZJ4uUS1CarG29Rz",
    "previous_digest": "25xTp3tzPbsMBhgKBzfbFYuZttjYyiXo32NquNvHfurd",
    "epoch": 423,
    "total_gas_cost": 1514372832436,
    "total_computation_cost": 38675200000,
    "total_storage_cost": 2466342165600,
    "total_storage_rebate": 990644533164,
    "non_refundable_storage_fee": 10006510436,
    "num_of_transaction_blocks": 1,
    "network_total_transactions": 1233295259,
    "timestamp": "2024-07-07T03:53:02+00:00",
    "checkpoint_commitments": "[]",
    "validator_signature": "iABlBGSu5MI9M6DOKcnXKeGNpQ97c0RtRttljSiFiwxI/DGNMiFWvkumDSvRNHS7",
    "end_of_epoch": FALSE,
    "end_of_epoch_data": "",
    "process_time": "2024-07-11T09:08:01+00:00",
    "block_date": "2024-07-07"
} 
```

## Topic projects/zettablock-data/topics/sui_mainnet_events_raw

Schema:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| checkpoint_digest            | String                | Checkpoint sequence digest                                                       |
| event_seq                    | Int                   | Sequence number of the event                                                     |
| sender                       | String                | Event sender address                                                             |
| package_id                   | String                | Package ID                                                                       |
| transaction_module           | String                | Transaction module                                                               |
| type                         | String                | Event type                                                                       |
| bcs                          | String                | Event bcs                                                                        |
| parsed_json                  | String                | Parsed event json string                                                         |
| block_time                   | String                | Block time                                                                       |
| block_date                   | String                | Block date                                                                       |
| process_time                 | String                | Process time                                                                     |


Attributes:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| event_seq                    | Int                   | Sequence number of the event                                                     |
| sender                       | String                | Event sender address                                                             |
| package_id                   | String                | Package ID                                                                       |
| transaction_module           | String                | Transaction module name                                                          |
| type                         | String                | Event type                                                                       |
| block_date                   | String                | Block date                                                                       |


Sample message:
```json
{
    "transaction_block_digest": "GsR6VNSerVgXgeTBv9MiMJepY64ZTSAbpCLB476CZDpj",
    "checkpoint_sequence_number": 83155874,
    "checkpoint_digest": "DNxp2BQVZgnNvSDQWq1cMfBmgr8NxZd9EjEUVjsDF5kJ",
    "event_seq": 0,
    "sender": "0xcd16172d6f2a216707c779e72e728d0a7a74119c26d61877d3bec81007a52158",
    "package_id": "0x000000000000000000000000000000000000000000000000000000000000000b",
    "transaction_module": "bridge",
    "type": "0x000000000000000000000000000000000000000000000000000000000000000b::bridge::TokenDepositedEvent",
    "bcs": "8sXB83VNuE5eCnKSrk5PS6s2Sfmxgy7mx1UknHyPSUsFW6kBzTz3XBN4Hd51cFn6NcAdN43qvqP527BxJrEDE3K33VmuAqcJgsif",
    "parsed_json": "{\"amount\": \"15000000\", \"sender_address\": [205, 22, 23, 45, 111, 42, 33, 103, 7, 199, 121, 231, 46, 114, 141, 10, 122, 116, 17, 156, 38, 214, 24, 119, 211, 190, 200, 16, 7, 165, 33, 88], \"seq_num\": \"53624\", \"source_chain\": 1, \"target_address\": [188, 143, 24, 110, 73, 178, 146, 128, 111, 102, 215, 111, 2, 130, 100, 225, 216, 108, 24, 239], \"target_chain\": 11, \"token_type\": 2}",
    "block_time": "2024-07-07T04:07:03+00:00",
    "process_time": "2024-07-11T09:19:51+00:00",
    "block_date": "2024-07-07"
} 
```

## Topic projects/zettablock-data/topics/sui_mainnet_object_changes_raw

Schema:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| checkpoint_digest            | String                | Checkpoint sequence digest                                                       |
| block_time                   | String                | Block time                                                                       |
| index                        | Int                   | Index of the object change                                                       |
| type                         | String                | Object change type                                                               |
| sender                       | String                | Object change sender address                                                     |
| owner_type                   | String                | Owner type                                                                       |
| owner_address                | String                | Owner address                                                                    |
| initial_shared_version       | Long                  | Initial shared version                                                           |
| object_type                  | String                | Object type                                                                      |
| object_id                    | String                | Object ID                                                                        |
| version                      | Long                  | Version number                                                                   |
| previous_version             | Long                  | Previous version number                                                          |
| digest                       | String                | Object change digest                                                             |
| package_id                   | String                | Package ID                                                                       |
| modules                      | Array(String)         | Module names                                                                     |
| block_date                   | String                | Block date                                                                       |
| process_time                 | String                | Process time                                                                     |


Attributes:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| index                        | Int                   | Index of the object change                                                       |
| type                         | String                | Object change type                                                               |
| sender                       | String                | Object change sender address                                                     |
| owner_type                   | String                | Owner type                                                                       |
| owner_address                | String                | Owner address                                                                    |
| object_type                  | String                | Object type                                                                      |
| object_id                    | String                | Object ID                                                                        |
| package_id                   | String                | Package ID                                                                       |
| block_date                   | String                | Block date                                                                       |


Sample message:
```json
{
    "transaction_block_digest": "EUpjqViLUr8tNapM3MNJfN1eG692sTWSSeMQxqQFTjS",
    "checkpoint_sequence_number": 22362217,
    "checkpoint_digest": "X8NqvdBwcYDsWZL5tdDoPbSEpePAzFS8uWmMaPG6vtG",
    "block_time": "2024-01-01T00:52:16+00:00",
    "index": 0,
    "type": "mutated",
    "sender": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "owner_type": "Shared",
    "owner_address": "",
    "initial_shared_version": 1,
    "object_type": "0x2::clock::Clock",
    "object_id": "0x0000000000000000000000000000000000000000000000000000000000000006",
    "version": 22362218,
    "previous_version": 22362217,
    "digest": "GuipZ4FvQqAV1paveAfmDXncMenobk53otyoHbY1h6xs",
    "package_id": "",
    "modules": [],
    "process_time": "2024-07-15T06:49:59+00:00",
    "block_date": "2024-01-01"
} 
```

## Topic projects/zettablock-data/topics/sui_mainnet_transaction_blocks_raw

Schema:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| digest                       | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| checkpoint_digest            | String                | Checkpoint sequence digest                                                       |
| block_time                   | String                | Block time                                                                       |
| index                        | Int                   | Index in the checkpoint                                                          |
| sender                       | String                | Sender address of the transaction block                                          |
| kind                         | String                | Transaction block kind                                                           |
| status                       | String                | Transaction block status                                                         |
| transaction_error            | String                | Transaction block error message                                                  |
| gas_object_id                | String                | Gas object ID                                                                    |
| gas_object_version           | Long                  | Gas object version number                                                        |
| gas_object_digest            | String                | Gas object digest                                                                |
| gas_owner                    | String                | Owner who pay the gas fee                                                        |
| gas_price                    | Long                  | Gas price                                                                        |
| gas_budget                   | Long                  | Gas budget                                                                       |
| tx_signatures                | Array(String)         | Transaction signatures                                                           |
| computation_cost             | Long                  | Computation cost                                                                 |
| storage_cost                 | Long                  | Storage cost                                                                     |
| storage_rebate               | Long                  | Storage rebate                                                                   |
| non_refundable_storage_fee   | Long                  | Non-refundable storage fee                                                       |
| total_gas_cost               | Long                  | Total gas cost                                                                   |
| num_of_transactions          | Int                   | Number of transactions                                                           |
| num_of_events                | Int                   | Number of events                                                                 |
| num_of_object_changes        | Int                   | Number of object changes                                                         |
| num_of_balance_changes       | Int                   | Number of balance changes                                                        |
| process_time                 | String                | Process time                                                                     |
| block_date                   | String                | Block date                                                                       |


Attributes:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| digest                       | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| checkpoint_digest            | String                | Checkpoint sequence digest                                                       |
| index                        | Int                   | Index in the checkpoint                                                          |
| sender                       | String                | Sender address of the transaction block                                          |
| kind                         | String                | Transaction block kind                                                           |
| status                       | String                | Transaction block status                                                         |
| gas_object_id                | String                | Gas object ID                                                                    |
| gas_owner                    | String                | Owner who pay the gas fee                                                        |
| block_date                   | String                | Block date                                                                       |



Sample message:
```json
{
    "digest": "3WZL6ghYt33UJhTR4aUcsbka2AUpiN29NYFHqJEMnyo3",
    "checkpoint_sequence_number": 39030595,
    "checkpoint_digest": "22ayfnkgMQrektkLipVeeYYF4UGJtCn4AVUmi1Xpj1Uf",
    "block_time": "2024-07-11T07:57:07+00:00",
    "index": 15,
    "sender": "0x7b72f93cf0b1081b29295208b0481254f4c710c4e32f91261b3c162f0e90214d",
    "kind": "ProgrammableTransaction",
    "status": "success",
    "transaction_error": "",
    "gas_object_id": "0x9a17de692e65dc36df85b23b196758c595c2badddc8aea552202d10c692dcadd",
    "gas_object_version": 296923309,
    "gas_object_digest": "BQA6ataM3BFRrkm7mo3ueVKLnfvqUGQXvmwWqYCRyrjE",
    "gas_owner": "0x39b9e5942df9a4686ebe727534077281d6adcee15a9bb74d3e052e56d78b2744",
    "gas_price": 758,
    "gas_budget": 4230796,
    "tx_signatures": ["ATxQpDE5EuMk7ygjrUKwyM/ZVr9+7ppT5LIEaDVQ6rFqEnkz+iB/rgde702dUKi6UFvsqdCnl5e4MFgmUKkcL88CIac5tgY6hCRVwTtlbHzD5JBbsPtG9mfAEMcc5cTHbIg=",
    "APSo80KTDU3PbuKcoBZAwyGdqerD7hYbdOBwej4GiSR1lHUUf2c6z4G7NedGqDpHim/x4pnGdZYZyDx33ldowg1UQp3QTxJiF8NVCzmLkBmvuSg0OwKb7vim23LJ3n8ryw=="],
    "computation_cost": 758000,
    "storage_cost": 5882400,
    "storage_rebate": 4145724,
    "non_refundable_storage_fee": 41876,
    "total_gas_cost": 2494676,
    "raw_transaction": "AQAAAAAAAgEBUJtR2PVJ5b6jVTrCDvUXq2aK19XQNud/3uDvO4vTbcjacUIJAAAAAAEBAWreufjQoshPMH8G/ufpwtfgQe7W52GGV4D2xBMXZ4LHCGsUBQAAAAABAQDaEtYhFp2pLtivX2szK3vsZMhAu0m7PUIG1nOc12utFAZ4ZmFudHYLY2xhaW1fdG9rZW4AAgEAAAEBAHty+TzwsQgbKSlSCLBIElT0xxDE4y+RJhs8Fi8OkCFNAZoX3mkuZdw234WyOxlnWMWVwrrd3IrqVSIC0QxpLcrd/ABHEQAAAAAgxqlSNaTu+SpEFeezbCSXwVmyRq9qD5+yQsoItxFbSH85ueWULfmkaG6+cnU0B3KB1q3O4Vqbt00+BS5W14snRPYCAAAAAAAAjI5AAAAAAAABxwEAAAAAAAACYgE8UKQxORLjJO8oI61CsMjP2Va/fu6aU+SyBGg1UOqxahJ5M/ogf64HXu9NnVCoulBb7KnQp5eXuDBYJlCpHC/PAiGnObYGOoQkVcE7ZWx8w+SQW7D7RvZnwBDHHOXEx2yIYQD0qPNCkw1Nz27inKAWQMMhnanqw+4WG3TgcHo+BokkdZR1FH9nOs+BuzXnRqg6R4pv8eKZxnWWGcg8d95XaMINVEKd0E8SYhfDVQs5i5AZr7koNDsCm+74pttyyd5/K8s=",
    "transaction_content": "",
    "effects_content": "",
    "num_of_transactions": 1,
    "num_of_events": 1,
    "num_of_object_changes": 4,
    "num_of_balance_changes": 1,
    "process_time": "2024-07-11T07:57:09+00:00",
    "block_date": "2024-07-11"
} 
```

## Topic projects/zettablock-data/topics/sui_mainnet_transactions_raw

Schema:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| checkpoint_digest            | String                | Checkpoint sequence digest                                                       |
| block_time                   | String                | Block time                                                                       |
| index                        | Int                   | Index in the transaction                                                         |
| type                         | String                | Transaction call type                                                            |
| data                         | String                | Transaction call data                                                            |
| package                      | String                | Transaction call package                                                         |
| module                       | String                | Transaction call module                                                          |
| function                     | String                | Transaction call function name                                                   |
| arguments                    | Array(String)         | Transaction call arguments                                                       |
| type_arguments               | Array(String)         | Transaction call type arguments                                                  |
| process_time                 | String                | Process time                                                                     |
| block_date                   | String                | Block date                                                                       |


Attributes:
| Name                         | Type                  | Description                                                                      |
|:-----------------------------|:----------------------|:---------------------------------------------------------------------------------|
| transaction_block_digest     | String                | Transaction block digest                                                         |
| checkpoint_sequence_number   | Long                  | Checkpoint sequence number                                                       |
| index                        | Int                   | Index in the transaction                                                         |
| type                         | String                | Transaction call type                                                            |
| package                      | String                | Transaction call package                                                         |
| module                       | String                | Transaction call module                                                          |
| function                     | String                | Transaction call function name                                                   |
| block_date                   | String                | Block date                                                                       |



Sample message:
```json
{
    "transaction_block_digest": "JBCrf8wrZh8SAq7uPppUNn1ZX8unpzUwk9b6rPCtQnjx",
    "checkpoint_sequence_number": 39028525,
    "checkpoint_digest": "AbxepQZYop7Yv7K6sgNnUnxpV1okReMb5koNi2rk9xvh",
    "block_time": "2024-07-11T07:22:46+00:00",
    "index": 6,
    "type": "MoveCall",
    "data": "{\"package\": \"0x04e20ddf36af412a4096f9014f4a565af9e812db9a05cc40254846cf6ed0ad91\", \"module\": \"hot_potato_vector\", \"function\": \"destroy\", \"type_arguments\": [\"0x4e20ddf36af412a4096f9014f4a565af9e812db9a05cc40254846cf6ed0ad91::price_info::PriceInfo\"], \"arguments\": [{\"NestedResult\": [5, 0]}]}",
    "package": "0x04e20ddf36af412a4096f9014f4a565af9e812db9a05cc40254846cf6ed0ad91",
    "module": "hot_potato_vector",
    "function": "destroy",
    "arguments": ["{\"NestedResult\": [5, 0]}"],
    "type_arguments": ["0x4e20ddf36af412a4096f9014f4a565af9e812db9a05cc40254846cf6ed0ad91::price_info::PriceInfo"],
    "process_time": "2024-07-11T07:22:48+00:00",
    "block_date": "2024-07-11"
} 
```

