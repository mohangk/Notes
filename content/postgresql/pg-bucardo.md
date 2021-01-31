---
draft: true
title: Pg Bucardo
categories:
  - Postgresql
---
# Bucardo setup on CloudSQL PostgreSQL

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
   1. pgbench -Upostgres -h10.128.0.2 -i
   pgbench -Upostgres -h10.128.0.2 -i

4. setup bucardo to sync data from self hosted to cloud sql postgres

## How burcardo works

- Perl daemon that listens for NOTIFY requests and acts on them by connecting to remove database and copying data back and forward
- add 2 or more databases and the tables to be synced to the main bucardo database 
- Add syncs: named replication actions, copying specific set of tables from one server to another server or group of servers



### Installation

1. Install dependecies

```bash
sudo apt-get install libdbi-perl libdbd-pg-perl libboolean-perl wget build-essential libreadline-dev libz-dev autoconf bison libtool  libproj-dev libgdal-dev libxml2-dev libxml2-utils xsltproc docbook-xsl docbook-mathml libossp-uuid-dev libperl-dev libdbix-safe-perl libencode-locale-perl libcgi-pm-perl
```

2. Checkout the code
```bash
git clone git@github.com:bucardo/bucardo.git
```
3. Install as per the README.md. To avoid installing (so that you can easily edit the source code in the same directory), do the following to ensure that the Bucardo.pm file can be found by Perl
```bash
export PERL5LIB=/path/to/bucardo/src
```
### Setup a sync

1. Setup bucardo on the bucardo postgresql instance

2. Ensure that both source and destination have the same schema

3. Add relevant envvars to help with setup

```bash
export SOURCE_HOST=10.83.64.3
export SOURCE_PORT=5432
export SOURCE_DATABASE=postgres
export SOURCE_USERNAME=postgres
export SOURCE_PASSWORD=password
export DEST_HOST=10.83.64.5
export DEST_PORT=5432
export DEST_DATABASE=postgres
export DEST_USERNAME=postgres
export DEST_PASSWORD=password
export TABLES="-t public.pgbench_accounts -t public.pgbench_branches -t public.pgbench_tellers"
export TABLES_WITH_SPACES="public.pgbench_accounts public.pgbench_branches public.pgbench_tellers"
```

2. Add the source and destination databases

./bucardo add db source_db dbhost=$SOURCE_HOST dbport=$SOURCE_PORT dbname=$SOURCE_DATABASE dbuser=$SOURCE_USERNAME dbpass=$SOURCE_PASSWORD
./bucardo add db dest_db dbhost=$DEST_HOST dbport=$DEST_PORT dbname=$DEST_DATABASE dbuser=$DEST_USERNAME dbpass=$DEST_PASSWORD

3. Add the tables

./bucardo add tables $TABLES_WITH_SPACES db=source_db

4. Add the herd

./bucardo add herd copying_herd $TABLES_WITH_SPACES

4. Add the sync

./bucardo add sync the_sync relgroup=copying_herd dbs=source_db:source,dest_db:target onetimecopy=2

5. Start the sync

./bucardo start

### Options
- onetimecopy - 0 (don't do anything), 1 (copy ), 2 (copy if its empty)
  - https://bucardo.org/onetimecopy/

- quick_delta_check = 0
   - Recommended to be disabled on busy tables - https://bucardo.org/pipermail/bucardo-general/2017-May/002896.html

-  bucardo_vac = 1

### Debugging 

1. What needs to be done if the database schema is changed,
   The bucardo schema on the source database will need to be reinitialised. This can be done by dropping the bucardo schema and letting bucardo reiniatilise it on sync start
   ```sql
   DROP SCHEMA bucardo CASCADE
   ```


References:

[Migrating legacy PostgreSQL databases to Amazon RDS or Aurora PostgreSQL using Bucardo | AWS Database Blog](https://aws.amazon.com/blogs/database/migrating-legacy-postgresql-databases-to-amazon-rds-or-aurora-postgresql-using-bucardo/)

[Using Bucardo 5.3 to Migrate a Live PostgreSQL Database - Compose Articles](https://www.compose.com/articles/using-bucardo-5-3-to-migrate-a-live-postgresql-database/)

Information on session replication role:
https://www.endpoint.com/blog/2015/01/28/postgres-sessionreplication-role

[Recommendation: set session_replication_role only if user is superuser · Issue #156 · bucardo/bucardo · GitHub](https://github.com/bucardo/bucardo/issues/156)
