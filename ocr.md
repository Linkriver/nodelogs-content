## OCR 

The off-chain reporting protocol enables features like decentralized data feeds by empowering Chainlink nodes to generate signed reports of aggregated data within an off-chain peer-to-peer network. Each Chainlink node aggregates data from a set of data sources, the individual observations are then compared, sorted and finally approved by all participating oracles. If the predefined conditions are met (e.g. minimum price deviation or maximum elapsed time), the final report is transmitted on-chain by one single Chainlink node, the aggregator smart contract then verifies that it was signed by a quorum of oracles, so that the median value of the report can be exposed to consuming contracts. 
The main purpose of the log messages in this section is to provide a better understanding of the functioning of the OCR protocol and the different roles a single Chainlink node can take.

[Official Chainlink documentation](https://docs.chain.link/docs/off-chain-reporting/)

[Chainlink Labs OCR research paper](https://research.chain.link/ocr.pdf)

## [ERROR] Pacemaker: Timeout while restoring state from database
OCR reports are generated in epochs, the pacemaker algorithm drives the report generation by keeping track of the state and message handling of an oracle to ensure continuous progress of the OCR protocol.
- Check the health and availability of the PostgreSQL server

## [ERROR] Pacemaker: error while restoring state from database
- Check the health and availability of the PostgreSQL server

## [WARN] non-leader round sender
The report generation follower algorithm manages observation requests received from the leader of the current report generation instance. In this case the leader seems to be incorrect, thus the protocol halts as an oracle may be trying to usurp the lead.
- Ensure the integrity of the network participants 

## [WARN] out of bounds round round rMax msgRound
This message warns of a potentially malicious leader as the round value is unusually high and should prevent denial-of-service attacks, the oracles exit the report generation protocol and emit a `changeleader` event.
- Ensure the integrity of the network participants 

## [ERROR] messageObserveReq: could not make SignedObservation observation
After receiving an observe request the oracle collects a new observation (calls its external adapters to fetch the needed data) and sends a signed message to the leader. The observation needs to be signed with the `offChainPrivateKey` of the oracle in order to produce a valid observe message for which it gets paid. 

## [ERROR] MakeSignedObservation produced invalid signature
This error indicates an issue with the `offChainPrivateKey` of the node which is necessary to produce a valid observe message for which the oracle gets paid. 

## [WARN] messageReportReq after report sent
If the report generation leader has received sufficient valid observe messages for a certain round, it sorts the observations by their values  and sends a report request message to the report generation follower nodes. In this case a report has already been sent and will not be submitted again.

## [WARN] messageReportReq after round completed
The report request will not be processed further because the round is no longer up-to-date, which may result from network delays.

## [ERROR] messageReportReq: could not validate report sent by leader
The Chainlink node could not validate that all signatures in the received report are valid.

## [ERROR] messageReportReq: failed to sign report
The Chainlink node could not validate the report containing the individual observations and oracle identities by signing the compressed report.

## [ERROR] ReportGeneration: DataSource errored
The Chainlink node could not get a successful response from its assigned data sources and may therefore not be able to send an observation to the current leader. 
- Make sure your external adapters are operational
- Make sure you did not hit your data provider rate limits
- Check if the used APIs are responsive

## [ERROR] shouldReport: blockchain interaction timed out, returning true
This error may indicate a deep reorg of the target network or issues with the remote RPC endpoint (full node) and prevents the Chainlink node from transmitting reports on-chain. However, it does not prevent the node from contributing to the report generation protocol.
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] shouldReport: Error during LatestTransmissionDetails
There may be too many reports created to determine whether a new final report should be sent, which depends on the predefined conditions of the data feed (e.g. price deviation, elapsed time since the last on-chain update).

## [ERROR] shouldReport: Error during LatestRoundRequested
There may be too many reports created to determine whether a new final report should be sent, which depends on the predefined conditions of the data feed (e.g. price deviation, elapsed time since the last on-chain update).

## [ERROR] messages not sorted by value
The observations were not sorted by value, which indicates issues during the report generation and could lead to incorrect results.

## [ERROR] given oracle ID of is out of bounds (only have public keys)
The report request errors as there is an invalid oracle ID not matching the number of known public keys.
- Ensure that untrustworthy participants are denied access to the peer-to-peer network

## [ERROR] not enough observations in report; got , need more than
In order to protect results against manipulation, a minimum number of oracles is required to report their observations in order to generate a valid report.
- Ensure that all Chainlink nodes that contribute to the protocol are protected against single points of failure and therefore highly available and responsive

## [WARN] verifyAttestedReport: dropping final report because it has too few signatures
- Make sure that all Chainlink nodes that contribute to the protocol are protected against single points of failures and therefore highly available and responsive

## [WARN] ReportGeneration: new round number would be larger than RMax + 1. Looks like your connection to more than f other nodes is not working.
The report generation protocol produces one report per round and repeats this process until it halts and the next protocol instance is started, this warning indicates delays of the peer-to-peer network and prevents the start of a new round.
- Make sure at least one bootstrap node is operational
- Make sure that all Chainlink nodes that contribute to the protocol are protected against single points of failures and therefore highly available and responsive

## [ERROR] ReportGeneration: round overflows, cannot start new round
There are too many rounds that did not end with a valid report, which may result from delays of the peer-to-peer network.
- Make sure at least one bootstrap node is operational
- Make sure that all Chainlink nodes that contribute to the protocol are protected against single points of failures and therefore highly available and responsive

## [WARN] Non-leader received MessageObserve
Observe messages are generated by each oracle participating as report generation follower and are exclusively sent to the current leader, who constructs a report after receiving the observations. This indicates irregularities in the message flow of the report generation leader protocol instance.

## [WARN] MessageObserve carries invalid SignedObservation
The report generation leader could not validate the signed observation sent by one of the oracles.

## [ERROR] leader's phase conflicts tGrace timeout
The `T_observe` grace period is started once the leader has received enough observations to construct a report and gives slower oracles the time to submit their observations. In this case the leader wants to move on to the report phase without the grace period being over.

## [WARN] messageReport: dropping MessageReport due to not being leader of the current epoch
The oracle that is supposed to send out the final report to get the signatures of the other participants is no longer the leader of the current epoch, either due to an insufficient number of progress events or the predetermined number of rounds being reached, which causes the pacemaker protocol to abort this report generation instance and start a new one.

## [WARN] messageReport: dropping MessageReport due to having already received sender's report
Messages that have been sent more than once may be a result of network delays.

## [ERROR] could not validate signature
The report generation leader could not validate the `OnChainSigningAddress` of the message sender, thus its identity cannot be verified and the report will not be used for the final attested report.

## [ERROR] contractEpoch() failed during eventTransmit
The transmission protocol handles the updating of the value that is stored in the aggregator contract by causing an oracle to send the final report through a transaction on-chain. Reports are only allowed if the aggregator contract has seen a report as recent as the latest incoming one to prevent similar transmissions. The latest contract epoch and thus the eligibility to transmit the final report could not be determined, which may be related to issues with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] Failed to serialize contract report
The latest report received by the aggregator contract determines the eligibility to transmit the final report, a failed serialization of it may be related to issues with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] could not compute median
The median of the latest incoming report must deviate by at least the predefined threshold from the median of the final report in order to be transmitted on-chain while avoiding needless transmissions, in this case it could not be computed.

## [ERROR] could not take median of observations
The median of the latest incoming report must deviate by at least the predefined threshold from the median of the final report in order to be transmitted on-chain while avoiding needless transmissions, in this case it could not be computed.

## [ERROR] Error while persisting pending transmission to database
A pending transmission cannot be written to the database.
- Check the health and availability of the PostgreSQL server

## [ERROR] Database.StorePendingTransmission timed out
- Check the health and availability of the PostgreSQL server

## [ERROR] eventTTransmitTimeout: Error while deleting pending transmission from database
- Check the health and availability of the PostgreSQL server

## [ERROR] Database.DeletePendingTransmission timed out
- Check the health and availability of the PostgreSQL server

## [ERROR] eventTTransmitTimeout: contractState() failed
The transmit event timed out as the contract state could not be determined, this may be related to issues with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] eventTTransmitTimeout: Transmit timed out
The transaction containing the final report could not be transmitted on-chain, this may be related to issues with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions

## [ERROR] eventTTransmitTimeout: Error while transmitting report on-chain
The transaction containing the final report could not be transmitted on-chain, this may be related to issues with the remote RPC endpoint (full node).
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions
