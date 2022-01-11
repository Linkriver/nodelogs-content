## HeadTracker

The HeadTracker stores the latest block number in a safe manner and reconstitutes the last block number from the data store on reboot.

## [ERROR] Error in new head subscription, unsubscribed
The Chainlink node is not able to create a new head subscription through the JSON-RPC method `eth_subscribe`. This issue might be related to the remote RPC endpoint (full node). 
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [WARN] HeadTracker: have not received a head for 
The Chainlink node has not received a head for the mentioned period of time. Block times can sometimes vary depending on the network and its conditions, so this log might be produced occasionally. The default period after which this log is produced is chain-specific for example: Ethereum mainnet: 1min, Polygon mainnet: 15sec
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] Headlistener: Failed to connect to ethereum node
The Chainlink node is not able to establish a websocket connection to the remote RPC endpoint (full node). This connection is established through the JSON-RPC method `eth_subscribe`.
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] ethereum ChainID doesn't match chainlink config.ChainID
- Check the `ETH_CHAIN_ID` setting of the Chainlink node
- Check if the Chainlink node is connected to the right RPC endpoint

## [ERROR] HeadTracker: got very old block with number (highest seen was ). This is a problem and either means a very deep re-org occurred, or the chain went backwards in block numbers. This node will not function correctly without manual intervention
- If the Chainlink node has previously synchronized another chain and stored the block headers in its database, it can be helpful to drop the database and re-initialize the node with the correct `ETH_URL` (make sure to backup the private key beforehand if necessary using the following commands)
```
chainlink admin login

chainlink keys eth list

chainlink keys eth export -p ./path/to/pw -o ./output/file.json <KeyID>
```
- Check the blockchain connection of the Chainlink node (Full-node-as-a-Service subscription and renew or switch the plan to prevent RPC rate limits from being hit if necessary)
- Run an own full node with custom configuration and no performance restrictions  
If you are running an own full node check
- Whether an error occurred requiring to resync the node (firewall, bad block, OOM)
- Whether it happens due to the node’s performance (hardware specs, disc’s IOPS)
