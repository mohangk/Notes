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
  1. Start replication  wal\_receiver sends the LSN up until the WAL data has been replayed on the replica to the primary\.
  1. The primary sends  wal\_sender the WAL data from the replica LSN till the latest LSN
  1. The replica writes the WAL data to WAL segments. The startup process on the the replica replays the data written to the WAL segment.

##### Concepts & config options

* different from logical replication, uses wal logs. A replica will read the WAL logs and restore it to get the changes from the primary
* warm-standby vs hot-standby
    * hot-stanby using streaming replication and can serve read-only queries
    * warm-standby - wal shipping - cannot accept any queries
    * both are running in recovery mode
* archive\_mode=off #default
    * When 'on' allows for the storage of WAL files that have already been checkpointed in a different location. Forms the basis of WAL shipping
* max\_wal\_sender=10 #default
    * the amount of replicas that can be streaming data from the primary. When a replica is streaming using pg\_basebackup - it uses 2 of these senders
* wal\_keep\_segments=0 #default
How many segments to keep for the purpose of the replicas. Not really required if WAL archiving is enabled
* wal\_log\_hints=off #default
server writes the entire content of each disk page to WAL during the first modification of that page after a checkpoint, even for non-critical modifications of so-called hint bits. Use for pg\_rewind

##### On the replica

* run `pg_basebackup` to copy the data over from the primary
  ` pg_basebackup -h 127.0.0.1 -U cary -p 5432 -D db-slave -P -Xs -R`
* Creates postgresql.auto.conf and standby.signal file




#### Common configuration

* hot\_standby= on #default
    * When on and there is a standby.signal file present, the server will run in Hot Standby mode.
* wal\_level = replica #default
* max\_wal\_senders

#### Quick setup

1. Setup
2. a. primary on 192.168.0.108
b. replica on 192.168.0.107
3. Install postgres on both instances.
4. Common configuration setting for both primayr and replica - postgresql.conf
a. Listen on external interfaces
`listen\_address=\*`
5. Common configuration - pg\_hba.conf
a. Allow connections from subnet
`host all all` 192.168.0.0`/24 md5`
6. Primary specific configuration
    1. Create a user for replication in
    `CREATE USER replicator WITH REPLICATION ENCRYPTED PASSWORD 'password'`
    2. Allow for replica to connect to master by adding it to the pg\_hba.conf.
    `echo "host replication replicator 192.168.0.107/32 md5" >> pg_hba.conf`
    Take not that instead of "all" or a database name, the keyword "replication" instead is used in the second column.
7. pg\_basebackup - stream the data through the wal sender process from master to a replica to setup replication. Either back up and restore or stream it by initiatiing it from the replica
    1. E.g. streaming
    pg\_basebackup -h 192.168.0.28 -U replicator -p 5432 -D $PGDATA -P -Xs -R
    Will automatically create recovery.conf
    2. Using tar backup - need to create your own recovery.conf
8. Once restored, start pg and it should resume where it last left off

#### Slots

* allows for WAL files to be stored on master

1. Key configuration on master (The default values work for replication so don't have to be changed)
    1. archive\_mode - enabled to archive WAL
    2. wal\_level -
        * `minimal` is default.
        * `replica` adds logging require for WAL archiving and read only queries on standby server.
        * `logical` adds information to allow extracting logical changes sets from WAL. Can start with 100
    3. wal\_keep\_segments - WAL retention in pg\_wal. Each WAL uses 16MB (unless Wal segment size has been changed)
    4. archive\_command - param takes a shell command or external program to copy the WAL segments to another location
    5. hot\_standby - \_must be set ot ON on the standby/replica - no effect on master. When replication is setup, the parameterts on the primary is automatically copied to replica. Important to have READS oin slcae.

#### pg\_basebackup

* Makes a binary backup of the database from primary to replica using the same protocol as the replication. Can also be done from a different replica (chained replica)
* Requires
    * SUPERUSER or REPLICATION privilegess
    * pg\_hba.conf must be set to explicitly permit replication connections
    * max\_wal\_senders need to be set high enough to allow for both backup and other time of WAL streaming

References

* https://www.postgresql.org/docs/10/app-pgbasebackup.html
* https://www.percona.com/blog/2018/09/07/setting-up-streaming-replication-postgresql/
* https://www.percona.com/blog/2018/11/30/postgresql-streaming-physical-replication-with-slots/
* https://www.percona.com/blog/2019/10/11/how-to-set-up-streaming-replication-in-postgresql-12/
* https://www.postgresql.org/docs/current/warm-standby.html
* https://en.wikibooks.org/wiki/PostgreSQL/Replication