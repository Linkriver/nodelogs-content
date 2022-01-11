## Keystore

This service enables the generation of new keys or the import, export and erasure of existing ones.

## [ERROR] key with ID already exists
During the first initialization, the Chainlink node generates a key itself, but you can also import a key from a JSON keystore file manually. This error indicates that you are trying to import an already existing key.
- Make sure to have followed the [official Chainlink documentation](https://docs.chain.link/docs/miscellaneous/#importing-a-keystore)
- If you want to delete an existing key do not forget to make a backup if necessary by exporting it using the following CLI commands
```
chainlink admin login 

chainlink keys eth list

chainlink keys eth export -p ./path/to/pw -o ./output/file.json <KeyID>
```
- Then delete the key using `chainlink keys eth delete <KeyID>`

## [ERROR] unable to find eth key with id
- Make sure to have followed the [official Chainlink documentation](https://docs.chain.link/docs/miscellaneous/#importing-a-keystore)
- Check the existing accounts by using the following CLI commands
```
chainlink admin login 

chainlink keys eth list 
```

## [WARN] No P2P_PEER_ID set, defaulting to first key in database
This message occurs if a Chainlink node is initialized with `FEATURE_OFFCHAINREPORTING=true` for the first time and no `P2P_PEER_ID` is set in the environmental file, so that a new ID is created and automatically used as default one.
- The `P2P_PEER_ID` can be found in the key section of the web GUI or displayed using the Chainlink CLI commands
```
chainlink admin login

chainlink keys p2p list
```

## [ERROR] multiple p2p keys found but peer ID was not set - you must specify a P2P_PEER_ID
The `P2P_PEER_ID` is used for OCR jobs and needs to be set in the environmental file in order to enable the communication of the Chainlink node within the peer-to-peer network.
