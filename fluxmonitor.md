## FluxMonitor

The FluxMonitor job type enables features like decentralized data feeds by empowering Chainlink nodes to read on-chain smart contract states (e.g. latest price stored in an aggregator contract) and examining a calculated median of freshly aggregated off-chain data for price deviations. Each node participating in such a decentralized oracle network gets the data by polling external adapters for different data provider APIs and submits the result on-chain when the predefined conditions are met.   
[Official Chainlink documentation](https://docs.chain.link/docs/jobs/types/flux-monitor/)

## [ERROR] unable to determine hibernation status
- Check the contract address
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] failed to get list of oracles from FluxAggregator contract
The Chainlink node needs to get the list of oracles that are allowed to submit observations to the AccesControlledAggregator in order to set the oracle address which matches the key of the node.
- Make sure to give access to all oracles that are supposed to submit to the aggregator contract by correctly adding the node addresses to the corresponding smart contract

## [WARN] None of the node's keys matched any oracle addresses, using first available key. This flux monitor job may not work correctly
- Make sure to give access to all oracles that are supposed to submit to the aggregator contract by correctly adding the node addresses to the corresponding smart contract

## [ERROR] Error determining if log was already consumed
It should be determined if a log is a duplicate of an already known one which may happen due to the backfilling feature and should result in ignoring this particular log. This error may occur after a node crash or restart, or indicate issues with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] Error determining if flag is still raised
The Chainlink node checks if both flags enabling the hibernation mode are lowered after receiving a `FlagsFlagRaised` log, this error indicates an issue with the remote RPC endpoint (full node). 
- Check the contract address
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] could not fetch oracleRoundState
The Chainlink node cannot check if a round has successfully closed with a new answer because it’s unable to consume the AnswerUpdated log. This may be related to an issue with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] error determining round stats / run status for round
The Chainlink node cannot determine the run status of a new round, this may be related to an issue with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] Ignoring new round request: error fetching eligibility from contract
The Chainlink node always checks its eligibility to submit to a new round by checking the round state, if it has already submitted to it and if the aggregator contract can pay it. In this case it is not able to fetch the needed data from the contract, which indicates an issue with the remote RPC endpoint (full node). 
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] error executing new run for job ID name 
- Make sure the on-chain configs and job spec are correct
- Make sure your external adapters are operational

## [WARN] LogBroadcaster is not connected to Ethereum node, skipping poll
The Chainlink node is not able to interact with the target network as it is not connected with a remote RPC endpoint (full node).
- Make sure to reestablish a functioning blockchain connection
- Check out existing automated failover solutions for the remote RPC endpoint of the Chainlink node, for example: [Fiews/ChainlinkEthFailover](https://github.com/Fiews/ChainlinkEthFailover)
