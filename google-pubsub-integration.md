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

For example, to monitor all USDT transfers, use the following filter on ethereum_token_transfers_raw topic:

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



## Topic projects/zettablock-data/topics/sui_mainnet_checkpoints_raw
## Topic projects/zettablock-data/topics/sui_mainnet_events_raw
## Topic projects/zettablock-data/topics/sui_mainnet_object_changes_raw
## Topic projects/zettablock-data/topics/sui_mainnet_transaction_blocks_raw
## Topic projects/zettablock-data/topics/sui_mainnet_transactions_raw

