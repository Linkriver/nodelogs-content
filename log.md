## Log

This service manages log subscription requests for the Chainlink node without creating a new subscription for each request and backfills logs in case of a node crash or restart. 

## [ERROR] Log subscriber could not create subscription to Ethereum node
The Chainlink node could not create a new log subscription to listen to on-chain events and will not be able to interact with the target network appropriately. This indicates an issue with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions  
If you are running an own full node check
- Whether an error occurred requiring to resync the node (firewall, bad block, OOM)
- Whether it happens due to the node’s performance (hardware specs, disc’s IOPS)

## [WARN] LogBroadcaster: BlockBackfillSkip is set to true, preventing a deep backfill - some earlier chain events might be missed
Backfilling guarantees to check older heads for relevant event logs in case the Chainlink node crashed or has been restarted. This error indicates that the Chainlink node will not get the event logs from older blocks and might skip potential job runs. `BlockBackfillSkip` is set to false by default to avoid this in case of a node crash or restart.
- [Official Chainlink documentation](https://docs.chain.link/docs/configuration-variables/#block_backfill_skip)

## [WARN] LogBroadcaster: Error in the event loop - will reconnect
The log subscription has been closed and could not be recreated, this might result from an issue with the remote RPC endpoint (full node) or a changed set of on-chain contracts the Chainlink node should listen to for events. 
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions
- Make sure your node address was added to the on-chain contracts it should interact with

## [WARN] LogBroadcaster: Detected a large block number difference between a log and recently seen head. This may indicate a problem with data received from the chain or major network delays
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions  
If you are running an own full node check
- Whether an error occurred requiring to resync the node (firewall, bad block, OOM)
- Whether it happens due to the node’s performance (hardware specs, disc’s IOPS)

## [ERROR] LogBroadcaster: Backfill - could not fetch latest block header, will retry
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions  
If you are running an own full node check
- Whether an error occurred requiring to resync the node (firewall, bad block, OOM)
- Whether it happens due to the node’s performance (hardware specs, disc’s IOPS)

## [ERROR] LogBroadcaster: Deadline exceeded, unable to backfill a batch of logs. Consider setting EvmLogBackfillBatchSize to a lower value
Blocks can be read in batches to avoid hitting the websocket request data limit if the number of blocks to check is high and differs significantly from the latest head. This may delay the processing of the latest log and the Chainlink node may miss job requests.
- Set `BlockBackfillSkip` to true if necessary and restart the node
- [Official Chainlink documentaiton](https://docs.chain.link/docs/configuration-variables/#eth_log_backfill_batch_size)

## [ERROR] LogBroadcaster: Inner deadline exceeded, unable to backfill a batch of logs. Consider setting EvmLogBackfillBatchSize to a lower value
- Set `EVM_LOG_BACKFILL_BATCH_SIZE` to a lower value, or
- Set `BlockBackfillSkip` to `true` if necessary and restart the node

## [ERROR] LogBroadcaster: Unable to backfill a batch of logs after retries
- Set `EVM_LOG_BACKFILL_BATCH_SIZE` to a lower value, or
- Set `BlockBackfillSkip` to `true` if necessary and restart the node
