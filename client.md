## Client

The client interface enables interactions with a remote RPC endpoint (full node) and thus the target blockchain a Chainlink node should interact with, it captures the methods the internal Geth client can perform.

## [ERROR] ethereum url scheme must be websocket
The websocket address of the remote RPC endpoint used by the Chainlink node must be set correctly in order to establish a working connection with the target network.
- Make sure the `ETH_URL` scheme starts with `ws://` or `wss://`

## [ERROR] secondary ethereum rpc url scheme must be http(s)
The secondary RPC endpoint(s) must be specified correctly in order to relieve the primary full node when sending transactions.
- Make sure the scheme looks like this: `ETH_SECONDARY_URLS=http(s)://node.com/1,https://logs.com/2,...`

## [ERROR] Failed to dial primary client
`Dial()` creates a new internal client that connects to the remote RPC endpoint (full node) set with the `ETH_URL` environmental variable. It initiates the blockchain connection of the Chainlink node and automatically tries to reconnect if it is interrupted.
If this error message occurs the Chainlink node cannot be (re-)initialized as it does not have a working blockchain connection.
- Check `ETH_URL` for correctness 
- Check the functionality of the connection manually (e.g. by using a program like wscat to establish a `newHeads` subscription)
```
wscat -c ws://$RPC_URL

{"id": 2, "method": "eth_subscribe",  "params": [newHeads"]}
```

## [ERROR] Failed to dial secondary client
`Dial()` creates a new internal client that connects to the remote RPC endpoint(s) (full node) set with the `ETH_SECONDARY_URLS` environmental variable. 
- Check `ETH_SECONDARY_URLS` for correctness 
- Check the connection’s functionality manually (e.g. by sending a JSON-RPC request to the RPC endpoint)
```
curl -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest",true], "id":1}' http://$RPC_URL
```

## [WARN] secondary eth client returned error
If `ETH_SECONDARY_URLS` is set the Chainlink node broadcasts every transaction to the remote RPC endpoints in parallel. 
- Make sure to use different full nodes as primary and secondary ones
- Check the blockchain connection of the Chainlink node (e.g. the Full-node-as-a-Service subscription and renew or switch the plan if necessary to prevent RPC rate limits from being hit)
- Run an own full node with custom configuration and no performance restrictions
