#### Streaming replication

##### High lever overview of the process

###### Processes

* wal\_sender
    * Runs on the primary
* wal\_receiver
    * Runs on replica
* startup
    * Runs on the replica

###### Flow

1. Start replication wal\_receiver sends the LSN up until the WAL data has been replayed on the replica to the primary.
2. The primary sends wal\_sender the WAL data from the replica LSN till the latest LSN
3. The replica writes the WAL data to WAL segments. The startup process on the the replica replays the data written to the WAL segment.


##### On the replica

* run `pg_basebackup` to copy the data over from the primary
`pg_basebackup -h 127.0.0.1 -U cary -p 5432 -D db-slave -P -Xs -R`
* Creates postgresql.auto.conf and standby.signal file
* “standby.signal” – indicates the server should start up as a hot standby
* When the server is promoted the standby.signal file is removed

#### Quick setup

1. Setup
a. primary on 192.168.0.108
b. replica on 192.168.0.107
2. Install postgres on both instances.
3. Common configuration setting for both primayr and replica - postgresql.conf
a. Listen on external interfaces
`listen\_address=\*`
4. Common configuration - pg\_hba.conf
a. Allow connections from subnet
`host all all 192.168.0.0/24 md5`
5. Primary specific configuration
    1. Create a user for replication in
    `CREATE USER replicator WITH REPLICATION ENCRYPTED PASSWORD 'password'`
    2. Allow for replica to connect to master by adding it to the pg\_hba.conf.
    `host replication replicator 192.168.0.107/32 md5`
    Take note that instead of "all" or a database name, the keyword "replication" instead is used in the second column.
6. Get the data into the replica either by taking a backup and restoring (from a stopped instnace) or stream the data through the wal sender process from master to a replica using `pg_basebackup` to setup replication.
    1. Using pg\_basebackup to copy the files and start streaming
    `pg_basebackup -h 192.168.0.28 -U replicator -p 5432 -D $PGDATA -P -Xs -R`
    Will automatically create postgresql.auto.conf and standby.signal
    2. Using tar backup - need to create your own postgresql.auto.conf and standby.signal file
7. Once restored, start pg and it should resume where it last left off

#### Validating the state of the replication

1. There will be walsender on the primary and walreceiver process on the replica
2. Use the `pg_stat_replication` table to get a state of the replication

#### Slots

* allows for WAL files to be stored on master for each replica that is streaming from it

1. On master create a slot by
`select * from pg_create_physical_replication_slot('replica')`
2. Configure replica to use the slot by setting the following in postgresql.conf. Requires stop and start of server.
`primary_slot_name = 'replica'`


#### recovery.conf values
- The dedicated recovery.conf file isnot used anymore, but this config options still existin in the  postgresql.conf file
primary_conninfo = 'user=replicator passfile=/tmp/pgpass host=10.128.15.244 port=5432 sslmode=prefer application_name=pg-patroni4-1 gssencmode=prefer'
recovery_target = ''
recovery_target_lsn = ''
recovery_target_name = ''
recovery_target_time = ''
recovery_target_timeline = 'latest'
recovery_target_xid = ''

- only one recovery_target option should be set 

- Replication configuration settings may be present even on primary servers
Any replication configuration settings (e.g. “primary_conninfo”) configured will be read by PostgreSQL and will be visible as normal, but their presence does not indicate whether the node is a standby or not. It is the existance of the standby.signal file that determines that

References:

* https://www.2ndquadrant.com/en/blog/replication-configuration-changes-in-postgresql-12/
* https://www.postgresql.org/docs/10/app-pgbasebackup.html
* https://www.highgo.ca/2019/11/07/streaming-replication-setup-in-pg12-how-to-do-it-right/
* https://www.percona.com/blog/2018/11/30/postgresql-streaming-physical-replication-with-slots/
* https://www.percona.com/blog/2019/10/11/how-to-set-up-streaming-replication-in-postgresql-12/
* https://www.postgresql.org/docs/current/warm-standby.html
* https://en.wikibooks.org/wiki/PostgreSQL/Replication

### Promotion of replica to primary

pg_ctl promote -D /path/to/data-dir

To trigger failover of a log-shipping standby server, run pg_ctl promote, call pg_promote(), or create a trigger file with the file name and path specified by the promote_trigger_file. If you're planning to use pg_ctl promote or to call pg_promote() to fail over, promote_trigger_file is not required. 

References: https://www.postgresql.org/docs/current/warm-standby-failover.html