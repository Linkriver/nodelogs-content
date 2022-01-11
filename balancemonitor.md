## BalanceMonitor

The BalanceMonitor checks the network's native token balance for each key on every new head the Chainlink node receives from the remote RPC endpoint.

## [ERROR] BalanceMonitor: error getting keys
The Chainlink node is not able to retrieve the keys for its available accounts from the keystore.
- Make sure you have followed the instructions for setting up a Chainlink node with  either newly generated or correctly imported keys from an existing account
- [Official Chainlink documentation](https://docs.chain.link/docs/miscellaneous/#importing-a-keystore)

## [ERROR] BalanceMonitor: error getting balance for key 
The Chainlink node is not able to retrieve the needed data object from the remote RPC endpoint it is connected to. The functioning of this service is mandatory for the monitoring of the Chainlink node account balance.
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions 
