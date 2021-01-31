---
draft: true
title: Standby_cluster
categories:
  - Patroni
tags:
  - databases
  - patroni
  - high-availability
---
## Creating a standby cluster

- Docs - https://patroni.readthedocs.io/en/latest/replica_bootstrap.html#standby-cluster
- Example standby cluster configuration:
```yaml
bootstrap:
  dcs:                                                                                                                                         ttl: 100
    loop_wait: 5
    retry_timeout: 10
    maximum_lag_on_failover: 1048576
    standby_cluster:
      host: 10.128.15.244
      port: 5432
      username: replicator
      password: password
```
## Replication slots
- By default the standby cluster will not use slots for replication, meanin it will rely entirely on the that log file retention set in in postgresql.conf as defined by (wal_keep_size)[https://www.postgresql.org/docs/13/runtime-config-replication.html]
- It might be better to explicity setup a replication slot, but that will require that the replication slot on the master be explicitly created 

## Common issues 

- Not having the postgresql.conf copied over as part of the pg_basebackup stage. When the 

## Promoting a standby cluster


run patronictl edit-config
remove the standby_cluster section from the config
Save and exit from the editor
review the config change and agree to apply it.

https://github.com/zalando/patroni/issues/1096
