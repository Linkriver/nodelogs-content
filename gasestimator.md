## GasEstimator

The GasEstimator can either run in `BlockHistory`, `FixedPrice` or `Optimism2` mode and manages the setting of the gas price for the Chainlink node’s outgoing transactions. The safest way to prevent stuck or overpaid transactions on most networks is to run the `BlockHistory` mode.

## [WARN] BlockHistoryEstimator: GAS_UPDATER_BLOCK_HISTORY_SIZE= is greater than ETH_FINALITY_DEPTH=, blocks deeper than finality depth will be refetched on every block history estimator cycle, causing unnecessary load on the eth node. Consider decreasing GAS_UPDATER_BLOCK_HISTORY_SIZE or increasing ETH_FINALITY_DEPTH
The BlockHistoryEstimator listens for new heads and updates the base gas price dynamically based on the configured percentile of gas prices in that block.

## [WARN] BlockHistoryEstimator: error fetching blocks
The Chainlink node is not able to fetch the block from its remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] BlockHistoryEstimator: cannot fetch, current block height is lower than GAS_UPDATER_BLOCK_DELAY=
The block delay variable determines the number of blocks that the block history estimator trails behind the head and thus which block to fetch. 
- Check the `BLOCK_HISTORY_ESTIMATOR_BLOCK_DELAY` setting and decrease it if necessary 
- Restart the node to apply the changes to the environmental file

## [WARN] BlockHistoryEstimator: cannot calculate percentile gas price
The Chainlink node cannot set a new default gas price as there are no suitable transactions in the block history to calculate the percentile gas price to choose.

## [WARN] Calculated gas price of Wei exceeds ETH_MAX_GAS_PRICE_WEI=, setting gas price to the maximum allowed value of Wei instead
The Chainlink node cannot set the new default gas price as its set maximum of `ETH_MAX_GAS_PRICE_WEI` would be exceeded. Transactions will never be sent with a higher gas price, the default values for this setting are chain-specific. 
- Check if the target network is facing any gas price abnormalities
- Increase `ETH_MAX_GAS_PRICE_WEI` if necessary 
- Decrease `BLOCK_HISTORY_ESTIMATOR_TRANSACTION_PERCENTILE`  if necessary
- Restart the node to apply the changes to the environmental file

## [WARN] Calculated gas price of Wei falls below ETH_MIN_GAS_PRICE_WEI=, setting gas price to the minimum allowed value of Wei instead
The Chainlink node cannot set the new default gas price as it would fall below its set minimum of `ETH_MIN_GAS_PRICE_WEI`. Transactions will never be sent with a lower gas price, the default values for this setting are chain-specific. 
- Decrease `ETH_MIN_GAS_PRICE_WEI` if necessary
- Increase `BLOCK_HISTORY_ESTIMATOR_TRANSACTION_PERCENTILE`  if necessary
- Restart the node to apply the changes to the environmental file
