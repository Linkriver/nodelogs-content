## Postgres

The Postgres service manages the connection and communication between the Chainlink node and its local or remote PostgreSQL database.

## [ERROR] could not get advisory lock for classID
The advisory lock provides a mechanism that locks the database as an application-oriented session without simultaneously preventing write operations (e.g. manual editing of the database). This means that other applications can be in the queue and have full access to the database as soon as the lock is released. It can be used for automated Chainlink node failover architectures by enabling a secondary node to wait for the lock to be released to jump in whenever the primary node fails (this practice is no longer recommended). This log indicates that the lock could not be closed.
- Make sure that the `ETH_DATABASE_URL` environmental variables of the primary and secondary Chainlink nodes are identical
- Check if the PostgeSQL database settings for the advisory lock is enabled

## [WARN] Postgres event broadcaster: disconnected, trying to reconnect…
The connection to the PostgreSQL database might have been interrupted and the Chainlink node tries to reconnect to resume the exchange with it. 
- Check the firewall settings of the Chainlink node’s host machine for outgoing traffic
- Check the firewall settings of the PostgreSQL server for incoming traffic
- Check the health and availability of the PostgreSQL server

## [WARN] Postgres event broadcaster: reconnect attempt failed, trying again…
The Chainlink node was not able to reconnect to the PostgreSQL database. It will produce this log until it can successfully reconnect.
- Check the firewall settings of the Chainlink node’s host machine for outgoing traffic
- Check the firewall settings of the PostgreSQL server for incoming traffic
- Check the health and availability of the PostgreSQL server
