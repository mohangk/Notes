---
draft: true
title: Replication Concepts
categories:
  - Postgresql
---
#### Postgres replications
##### Approaches
* https://www.percona.com/live/18/sites/default/files/slides/replication-perconalive.pdf
* shared disk failover
* block level replication
* WAL shipping
* Trigger based
* Logical
    * No out of the box DDL change support - [GitHub - enova/pgl\_ddl\_deploy: Transparent DDL replication using Pglogical](https://github.com/enova/pgl_ddl_deploy)
    
##### Concepts & config for native replication 

* different from logical replication, uses wal logs. A replica will read the WAL logs and restore it to get the changes from the primary
* warm-standby vs hot-standby
    * hot-stanby using streaming replication and can serve read-only queries
    * warm-standby - wal shipping - cannot accept any queries
    * both are running in recovery mode
* hot\_standby=on #default.
    * Has no effect on master. But on replica if standby.signal is available it will put the database into readonly mode and continuously stream WAL from primary
* archive\_mode=off #default
    * When 'on' allows for the storage of WAL files that have already been checkpointed in a different location. Forms the basis of WAL shipping
* archive\_command - param
    * takes a shell command or external program to copy the WAL segments to another location
* max\_wal\_sender=10 #default
    * the amount of replicas that can be streaming data from the primary. When a replica is streaming using pg\_basebackup - it uses 2 of these senders
* wal\_keep\_segments=0 #default
How many segments to keep for the purpose of the replicas. Not really required if WAL archiving is enabled
* wal\_log\_hints=off #default
server writes the entire content of each disk page to WAL during the first modification of that page after a checkpoint, even for non-critical modifications of so-called hint bits. Use for pg\_rewind
* wal\_level -
\* `minimal` is default.
\* `replica` adds logging require for WAL archiving and read only queries on standby server.
\* `logical` adds information to allow extracting logical changes sets from WAL. Can start with 100
* synchronous_commit=on #default
     * This configuration is used both to determine  the guarantees of the "transaction commit". On means that it will guarantee that WAL records were flushed to disk before the command returns succes  to the client.  Having it off - doesnt leave the database in an inconsistent state, but leaves clients in an inconsistent state. 
     * If synchronous_standby_names is non-empty, this parameter also controls whether or not transaction commits will wait for their WAL records to be replicated to the standby server(s).    
         *  **local** causes commits to wait for local flush to disk, but not for replicas. This defeats the purpose of using synchronous replication . This  is provided for completeness.
         * **remote_write**, commits will wait until replies from the current synchronous standby(s) indicate they have received the commit record of the transaction and written it out to their operating system - but not necessarily flushed to disk. This is a lower guarantee then **on** This setting is sufficient to ensure data preservation even if a standby instance of PostgreSQL were to crash, but not if the standby suffers an operating-system-level crash, since the data has not necessarily reached durable storage on the standby.
       *  **on**, commits will wait until replies from the current synchronous standby(s) indicate they have received the commit record of the transaction and flushed WAL record  to disk. This ensures the transaction will not be lost unless both the primary and all synchronous standbys suffer corruption of their database storage. 
         * **remote_apply**, Provides the strongest replica consistency. Commits will wait until replies from the current synchronous standby(s) indicate they have received the commit record of the transaction and applied it. Clients could either query the primary or the standby, they would have exactly the same view on the data.
References:
 - https://www.cybertec-postgresql.com/en/the-synchronous_commit-parameter/
 - https://www.2ndquadrant.com/en/blog/evolution-fault-tolerance-postgresql-synchronous-commit/
 

 ##### Determining replication lag

 References: 

 - https://severalnines.com/database-blog/what-look-if-your-postgresql-replication-lagging
 - https://stackoverflow.com/questions/28323355/how-to-check-the-replication-delay-in-postgresql




