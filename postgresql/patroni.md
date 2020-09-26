Patroni

replicat imaging and bootstrap

- allows customizing creation of new replica

- also allows defining what happends when new cluster is bootstrapped

- Patroni only creates replicas if the "initialize" key is present in the DCS. In no key, bootstrap is only called exclusively on the first node that that takes the initialize key lock

Bootstrap

- patroni calls initidb by default

- when creating a new cluster as a copy of existing cluster its necessary to do some custom action





initial copy using wal_g [How to properly restore cluster using WAL-G (WAL-E) to a specific time (PITR) on Postgres 12? · Issue #1571 · zalando/patroni · GitHub](https://github.com/zalando/patroni/issues/1571)_
