Streaming replication
:
Concepts

* different from logical replication, uses wal logs. A replica will read the WAL logs and restore it to get the changes from the primary
* Key processes
    * wal\_sender
        * Runs on the primary
    * wal\_receiver
        * Runs on replica
    * startup\_\_
        * Runs on the replica
* Process:
1\. Start replication \- wal\_receiver sends the LSN up until the WAL data has been replayed on the replica to the primary\.
2\. The primary sends \- wal\_sender\_ the WAL data from the replica LSN till the latest LSN
3\. The replica writes the WAL data to WAL segments\. The startup process on the the replica replays the data written to the WAL segment\.

Quick setup

1. Setup
a. primary on 192.168.0.108
b. replica on 192.168.0.107
2. Install postgres on both instances.
3. Common configuration - postgresql.conf
a. Listen on external interfaces
`listen\_address=\*`
4. Common configuration - pg\_hba.conf
a. Allow connections from subnet
`host all all ` 192.168.0.0`/24 md5`
5. Create a pg
6. \_Create a user for replication in Master
`CREATE USER replicator WITH REPLICATION ENCRYPTED PASSWORD 'password'`
7. Allow for replica to connect to master by adding it to the pg\_hba.conf. Take not that instead of "all" or a database name, the keyword "replication" instead is used in the second column.
echo "host replication relicator 192.168.0.107/32 md5" >> $PGDATA/pg\_hba.conf
8. pg\_basebackup - stream the data through the wal sender process from master to a replica to setup replication. Either back up and restore or stream it by initiatiing it from the replica
    1. E.g. streaming
    pg\_basebackup -h 192.168.0.28 -U replicator -p 5432 -D $PGDATA -P -Xs -R
    Will automatically create recovery.conf
    2. Using tar backup - need to create your own recovery.conf
9. Once restored, start pg and it should resume where it last left off

SLOTS

* allows for WAL files to be stored on master

1. Key configuration on master (default values so don't have to be changed)
    1. archive\_mode - enabled to archive wals
    2. wal\_level - minimal is default. replica adds logging require for WAL archiching and read only queries on standby server. logical adds information to allow extracting logical changes sets from WAL. Can start with 100
    3. wal\_keep\_setments - WAL retention in pg\_wal. Each WAL uses 16MB (unless Wal segment size has been changed)\_\_\_
    4. archive\_comnad - param takes a shell command or external program to compy the WAL segments to another location
    5. hot\_standby - \_must be set ot ON on the standby/replica - no effect on master. When replication is setup, the parameterts on the primary is automatically copied to replicat. Important to have READS oin slcae.

References
https://www.postgresql.org/docs/10/app-pgbasebackup.html
https://www.percona.com/blog/2018/09/07/setting-up-streaming-replication-postgresql/
https://www.percona.com/blog/2018/11/30/postgresql-streaming-physical-replication-with-slots/
https://www.percona.com/blog/2019/10/11/how-to-set-up-streaming-replication-in-postgresql-12/
https://www.postgresql.org/docs/current/warm-standby.html

[How To Configure PostgreSQL 12 Streaming Replication in CentOS 8](https://www.tecmint.com/configure-postgresql-streaming-replication-in-centos-8/)
