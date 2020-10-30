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

#####

##### On the replica

* run `pg_basebackup` to copy the data over from the primary
`pg_basebackup -h 127.0.0.1 -U cary -p 5432 -D db-slave -P -Xs -R`
* Creates postgresql.auto.conf and standby.signal file

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

References

* https://www.postgresql.org/docs/10/app-pgbasebackup.html
* https://www.highgo.ca/2019/11/07/streaming-replication-setup-in-pg12-how-to-do-it-right/
* https://www.percona.com/blog/2018/11/30/postgresql-streaming-physical-replication-with-slots/
* https://www.percona.com/blog/2019/10/11/how-to-set-up-streaming-replication-in-postgresql-12/
* https://www.postgresql.org/docs/current/warm-standby.html
* https://en.wikibooks.org/wiki/PostgreSQL/Replication