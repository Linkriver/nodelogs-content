## PeriodicBackups

The Chainlink node database can be backed up automatically at regular time intervals. [Official Chainlink documentation](https://docs.chain.link/docs/configuration-variables/#database_backup_mode)

## [ERROR] Invalid path for DATABASE_BACKUP_DIR - please set it to a valid directory path
- Make sure an existing directory path is used
- Default: Chainlink root directory 

## [ERROR] Database backup frequency is too small. Please set it to at least
- Set a positive regular time interval (at least 1m) for dumping the database
- Examples with valid time unit (e.g. „m“, „h“): `DATABASE_BACKUP_FREQUENCY=7h`

## [ERROR] DatabaseBackup: Failed
- Make sure a valid directory path is used
- Make sure there is enough disk space for storing the backup file
