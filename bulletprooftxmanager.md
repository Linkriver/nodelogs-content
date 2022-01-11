## BulletproofTxManager

This service is responsible for creating, if necessary rebroadcasting and eventually confirming all transactions that the Chainlink node sends on-chain.

## [WARN] BulletproofTxManager: dropped old transactions from transaction queue
The Chainlink node drops the oldest transactions as there are too many queued ones in order to create a new transaction. A large amount of non-broadcast transactions indicates an issue with the remote RPC endpoint (full node), or that a previous transaction is stuck. If the node is not able to rebroadcast it you may need to delete the unconfirmed transactions and pending job runs from the database manually.
- Run an own full node with custom configurations and no performance restrictions 
- Run these commands on your database to delete all unconfirmed transactions and pending job runs (always make a backup of your node and database before you manually change anything in the database!)
```
SELECT * FROM eth_txes;

DELETE from eth_txes WHERE state = 'unconfirmed';

DELETE from job_runs WHERE status = 'pending_outgoing_confirmations';
```

## [ERROR] cannot create transaction; too many unstarted transactions in the queue
The Chainlink node cannot create a new transaction as the queue limit is exceeded. Many queued transactions indicate that a previous transaction is stuck, if the node is not able to rebroadcast it you may need to delete the unconfirmed transactions and pending job runs from the database manually.
- Check the node’s `ETH_MAX_QUEUED_TRANSACTIONS` setting
- Run these commands on your database to delete all unconfirmed transactions and pending job runs (always backup your node and database before you manually change anything in the database!)
```
SELECT * FROM eth_txes;

DELETE from eth_txes WHERE state = 'unconfirmed';

DELETE from job_runs WHERE status = 'pending_outgoing_confirmations';
```


## [WARN] EthBroadcaster: transaction throttling; transactions in-flight and unstarted transactions pending (maximum number of in-flight transactions is per key)
The default value of maximum in-flight transactions is 16, so your Chainlink node may be struggling holding up with the frequency of incoming job requests. If the node successfully broadcasts transactions that do not get confirmed they could be underpriced and not being picked up by the network’s validators. If the node is not able to rebroadcast unconfirmed transactions you may need to delete these and the pending job runs from the database manually.
- Check the node’s `GAS_ESTIMATOR_MODE` setting
- Check the node’s other [gas related settings](https://docs.chain.link/docs/configuration-variables/#gas-controls)
- For high-throughput Chainlink nodes: configure your full node accordingly and increase ETH_MAX_IN_FLIGHT_TRANSACTIONS (Default: 16)
- Run these commands on your database to delete all unconfirmed transactions and pending job runs (always backup your node and database before you manually change anything in the database!)
```
SELECT * FROM eth_txes;

DELETE from eth_txes WHERE state = 'unconfirmed';

DELETE from job_runs WHERE status = 'pending_outgoing_confirmations';
```

## [ERROR] EthBroadcaster: transaction gas price was rejected by the eth node for being too high. 
The Chainlink node tried to submit a transaction with a gas price exceeding the set limit of its remote RPC endpoint (full node). 
- Check the Chainlink node’s blockchain connection (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions 
If you are running an own full node make sure to
- Consider to increase `RPCTxFeeCap`
- It is recommended to run Geth with no cap, i.e. `--rpc.gascap=0 --rpc.txfeecap=0`

## [ERROR] EthBroadcaster: tx at gas price Wei was rejected due to insufficient eth
The native token balance of the Chainlink node account is too low to submit the transaction.
- Fund the Chainlink node account with a sufficient amount of the network’s native token
- You can view the node’s account address when the node starts up or on the Keys page of the web GUI

## [ERROR] default gas price wei was rejected by the eth node for being too low
The Chainlink node tried to submit a transaction with an insufficient gas price.
- Check the `GAS_ESTIMATOR_MODE` setting
- Check the node’s other [gas related settings](https://docs.chain.link/docs/configuration-variables/#gas-controls)

## [ERROR] Hit gas price bump ceiling, will not bump further. This is a terminal error
The Chainlink node repeatedly bumped an unconfirmed transaction and will not continue to bump the gas price any further. This could be due to a network congestion or an internal bumping issue.
- Check the `GAS_ESTIMATOR_MODE` setting
- Increase `ETH_MAX_GAS_PRICE_WEI` if necessary
- Restart the node to apply the changes to the environmental file

## [ERROR] invariant violation: could not increment nonce because no rows matched query
The Chainlink node cannot increment the nonce of the account, this may result from a modification of the nonce while using the account with another Chainlink instance or an external wallet.
- Make sure not to send transactions from an account that is used by a Chainlink node except through the Chainlink CLI itself, otherwise it could lead to unrecoverable nonce issues

## [ERROR] EthConfirmer batchFetchReceipt failed 
The remote RPC endpoint used by the Chainlink node is not able to transfer the status of a broadcast transaction using the `eth_getTransactionReceipt` method. The Chainlink node needs the receipt to check if a transaction has already been mined or if further actions like resubmitting with a bumped gas price are necessary.
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions  
If you are running an own full node make sure to  
- Enable the `--txpool.locals` flag so that the full node can keep better track of the Chainlink node transactions that are sent to the mempool
- `--txpool.locals $CHAINLINK_NODE_ADDRESS`
- Increase `--txpool.globalslots` (Default: 2000)

## [WARN] Transaction reverted on chain
Reverted on-chain transactions are often related to chain reorgs and the settings for minimum incoming confirmations. The Chainlink node may submit multiple transactions if it does not find the previous ones in the mempool so that they become “stale”, this may be due to a chain reorg or issues with the remote RPC endpoint (full node). Reverted transactions can also occur if several nodes from the same OCR network transmit their results for a certain round in a short time interval and only the transaction that is confirmed first is not reverted.
- Check the `MIN_INCOMING_CONFIRMATIONS` setting
 (Default values have been tested and adjusted for each network, values < 3 may result in multiple transactions)
- Only set `ETH_HTTP_URL` if you run your own full node to be sure to use the same full node on the backend (`ETH_URL` and `ETH_HTTP_URL` need to point to the same full node)  
If you are running an own full node make sure to
- Enable the `--txpool.locals` flag and add the addresses of your Chainlink node(s)
- `--txpool.locals $CHAINLINK_NODE_ADDRESS`
- Test different settings for `--peers` depending on the network (Default = 100)
- Less peers: probably less, but deeper reorgs 
- More peers: probably more, but less deep reorgs 

## [ERROR] EthConfirmer: eth_tx with ID expired without ever getting a receipt for any of our attempts
Chainlink requires exclusive ownership of its private keys, sharing them across multiple Chainlink instances or using the keys with an external wallet is not supported and will lead to missed transactions.
- Make sure not to send transactions from an account that is used by a Chainlink node except through the Chainlink CLI itself, otherwise it could lead to unrecoverable nonce issues

## [WARN] EthConfirmer: transactions to rebroadcast which exceeds limit of 
Too many transactions need to be rebroadcast and the limit of in-flight transactions is exceeded, so the Chainlink node is struggling to hold up with the frequency of incoming job requests. If the node successfully broadcasts transactions that do not get confirmed they may be underpriced and not be picked up by the network’s validators.
- Check the `GAS_ESTIMATOR_MODE` setting
- Check the node’s other [gas related settings](https://docs.chain.link/docs/configuration-variables/#gas-controls)
- For high-throughput Chainlink nodes: configure the full node accordingly and increase `ETH_MAX_IN_FLIGHT_TRANSACTIONS` (Default: 16)

## [ERROR] Failed to bump gas
The Chainlink node does not bump the gas for an unconfirmed transaction, instead it tries to resubmit the previous attempt until it gets accepted.
- Check the `GAS_ESTIMATOR_MODE` setting
- Increase `ETH_MAX_GAS_PRICE_WEI` if necessary
- Restart the node to apply the changes to the environmental file
 
 ## [WARN] EthConfirmer: chain length supplied for re-org detection was shorter than EvmFinalityDepth. If this happens a lot, it could indicate a problem with the remote RPC endpoint, a compatibility issue with a particular blockchain, heads table being truncated too early, or some other problem
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions 
- Make sure the network the node is connecting to is supported by Chainlink

## [WARN] EthResender: failed to resend unconfirmed transactions
This message may occur if gas bumping is disabled or the network is experiencing abnormally long block times. It may also be related to issues with the remote RPC endpoint (full node).
- Check the `GAS_ESTIMATOR_MODE` setting
- Check the node’s other [gas related settings](https://docs.chain.link/docs/configuration-variables/#gas-controls)
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions 

## [WARN] NonceSyncer: address has been used before, either by an external wallet or a different Chainlink node. Local nonce is but the on-chain nonce for this account was. It's possible that this node was restored from a backup. If so, transactions sent by the previous node will NOT be re-org protected and in rare cases may need to be manually bumped/resubmitted. 
Chainlink requires exclusive ownership of its private keys, sharing them across multiple Chainlink instances or using the keys with an external wallet is not supported and will lead to missed transactions.
- Make sure not to send transactions from an account that is used by a Chainlink node except through the Chainlink CLI itself, otherwise it could lead to unrecoverable nonce issues
