1. setup cloudsql postgres

2. setup self hosted postgres
   
   1. ensure can connect from client instance 
   
   2. edit pg_hba.conf
      
      1. host    all    all    10.128.0.0/20    trust
   
   3. Listen on *
   
   4. Setup logging
   
   5. Increase amount of clients that can connect
   
   6. setup monitoring for key pg metrics to understand the performance
      
      1. Is bottleneck disk io, CPU or something else?
   
   7. optimise the config

3. run pg-bench on self hosted postgres
   
   1. pgbench -Upostgres -h10.128.0.2 -ipgbench -Upostgres -h10.128.0.2 -i

4. setup bucardo to sync data from self hosted to cloud sql postgres

#### How burcardo works

- Perl daemon that listens for NOTIFY requests and acts on them by connecting to remove database and copying data back and forward

- add 2 or more databases and the tables to be synced to the main bucardo database 

- Add syncs: named replication actions, copying specific set of tables from one server to another server or group of servers

##### Setting up bucardo

1. Ensure that both source and destination have the same schema

2. Add the databases

3. Add the tables

4. Add the sync

bucardo add sync benchdelta relgroup=alpha dbs=destination onetimecopy=1 

##### Additional

- update https://cloud.google.com/community/tutorials/setting-up-postgres

- look into using a seperate data disk running XFS

- look at setting up [GitHub - zalando/patroni: A template for PostgreSQL High Availability with Etcd, Consul, ZooKeeper, or Kubernetes](https://github.com/zalando/patroni)

- basic tuning of config

##### Operational modes

1. Doing PITR or snapshot updates

2. Upgrading the postgres version

3. Resizing disk

4. Regional disk

References:

[Migrating legacy PostgreSQL databases to Amazon RDS or Aurora PostgreSQL using Bucardo | AWS Database Blog](https://aws.amazon.com/blogs/database/migrating-legacy-postgresql-databases-to-amazon-rds-or-aurora-postgresql-using-bucardo/)

[Using Bucardo 5.3 to Migrate a Live PostgreSQL Database - Compose Articles](https://www.compose.com/articles/using-bucardo-5-3-to-migrate-a-live-postgresql-database/)

https://www.endpoint.com/blog/2015/01/28/postgres-sessionreplication-role

http://manpages.ubuntu.com/manpages/bionic/man1/bucardo.1p.html

[Recommendation: set session_replication_role only if user is superuser · Issue #156 · bucardo/bucardo · GitHub](https://github.com/bucardo/bucardo/issues/156)
