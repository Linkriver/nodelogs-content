## DirectRequest

The following log messages are related to the direct request model, which is commonly used in combination with an on-chain oracle or operator contract that the Chainlink node observes for emitted event logs.

## [ERROR] DirectRequest: OracleRequest log mailbox is over capacity - dropped the oldest log
If a consumer sends a Chainlink request to an oracle or operator contract an event log is emitted containing a job ID and other request parameters. The Chainlink node picks up the log and initiates a job run if the job ID is matching one of those in its database. This error message indicates that there is a high amount of requests and the Chainlink node is not able to process all related logs which could lead to missed job runs.
- Decrease the direct request frequency for this particular Chainlink node
- Do not list your node details publicly if you want to avoid an unpredictable amount of requests
- Use the following database commands to whitelist requesters for a single job (always  backup your node and database before you manually change anything in the database!)
```
SELECT * FROM initiators; 
UPDATE initiators SET requesters='$ADDRESS' WHERE job_spec_Id='$JOB_ID'; 
```
- Use the following database commands to whitelist requesters for all RunLog jobs
```
UPDATE initiators SET requesters='$ADDRESS1,$ADDRESS2,$ADDRESS3' WHERE type ='runlog';
```

## [ERROR] DirectRequest: failed executing run
The Chainlink node checks if the request was sent by a valid requester and if the payment is sufficient to initiate a job run. 
- Check the `MINIMUM_CONTRACT_PAYMENT_LINK_JUELS` setting of the Chainlink node or relevant job
- Use the following database commands to whitelist requesters for a single job (always backup your node and database before you manually change anything in the database!)
```
SELECT * FROM initiators; 
UPDATE initiators SET requesters='$ADDRESS' WHERE job_spec_Id='$JOB_ID'; 
```
- Use the following database commands to whitelist requesters for all RunLog jobs
```
UPDATE initiators SET requesters='$ADDRESS1,$ADDRESS2,$ADDRESS3' WHERE type ='runlog';
```
