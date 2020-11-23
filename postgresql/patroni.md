#### Patroni

* allows customizing creation of new replica
* also allows defining what happends when new cluster is bootstrapped
* Patroni only creates replicas if the "initialize" key is present in the DCS. If no key, bootstrap is only called exclusively on the first node that that takes the initialize key lock

##### Bootstrap

* Bootstrap happens when the data\_dir is empty. It involves
    * Creating users - postgres, replicator and other users defined in the bootstrap section
    * Once bootstrapped the `initialize` key will be defined in the DCS
    * Question: Why does it create a password for postgres and the replicator user although only admin is defined in the bootstrap section?
* when creating a new cluster as a copy of existing cluster its necessary to do some custom action

##### Initial copy

initial copy using wal\_g [How to properly restore cluster using WAL-G (WAL-E) to a specific time (PITR) on Postgres 12?](https://github.com/zalando/patroni/issues/1571)

##### Failure to start

* lack of required directories - add it to the config for it to be auto-created. Its worth adding this to the docs?
    * https://github.com/zalando/patroni/issues/863
    * https://github.com/zalando/patroni/pull/1293
* system identifier conflict
    * /usr/lib/postgresql/12/bin/pg\_controldata -D /var/lib/postgresql/12/main -- its part of the data directory, used as part of the initialize in DCS
    * Cluster was initialized during install of postgres packages
        * https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 128
* Ensure yaml file properly formatted
    * https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 127
* Ensure Patroni can find binaries
    * https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 124
* initdb configuration settings
    * https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 126
* failure to detect DCS
    * https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 123

##### Converting an existing PG instance to be the master of a Patroni cluster

https://patroni.readthedocs.io/en/latest/existing\_data.html - how to modify an existing pg instance into a patroni cluster
https://github.com/zalando/patroni/issues/1111

##### Reinitializing an existing patroni cluster

https://github.com/zalando/patroni/issues/1108

* Stop all your nodes by executing systemctl stop patroni; systemctl stop postgresql;
* change your working directory to /data/dir (cd /data/dir) if this directory is specified in postgresql section of patroni.yml and execute command rm -rf \* in this directory, WARNING! This command will delete all your databases, so don't run it if you don't want to lose your data
* Then, on any of your nodes to remove cluster information from ETCD, execute patronictl -c %path\_to\_your\_patroni.yml% remove %cluster\_name%. You can find your cluster name in patroni.yml, SCOPE string
* Then setup a third node, install postgres and patroni, create patroni service.
* When all nodes are up, execute systemctl start patroni; systemctl start postgresql on any and wait for 10-20 second to bootstrap cluster, then execute such command on another nodes.

##### How can pg\_hba.conf be configured?

https://stackoverflow.com/questions/57570581/can-you-change-pg-hba-conf-using-patronictl

https://github.com/zalando/patroni/issues/878

There are actually more places where you can configure it, just pick one and stick with it:

* bootstrap.pg\_hba
this is the oldest one. The value from here is appended to pg\_hba.conf right after initdb run and later never changed (even if you edit the config file)
* bootstrap.dcs.postrgesql.pg\_hba
value from here is written into /config key into DCS. After that any node will take this value and write it into pg\_hba.conf. Later you can use patronictl edit-config to change pg\_hba on all nodes in the cluster.
* postrgesql.pg\_hba
value from here will completely overwrite pg\_hba.conf upon restart or reload of patroni.
* postgresql.parameters.hba\_file.
If the hba\_file if set, all other pg\_hba parameters are ignored.
postrgesql.pg\_hba from the config file takes precedence over the value from the DCS:/config key.

It is not really possible to merge pg\_hba from different sources, because rules are applied line by line.

##### How does Patroni handle split brain?

https://github.com/zalando/patroni/issues/1114

##### What happens when a node is added or removed on the primary?

* slots are removed when nodes are removed either by shutting down or removing?

##### What happends when the DCS is inaccessible

```
* related: Can we avoid demoting primary when DCS is not available?
```

DCS is a source of truth about running leader
\* DCS keeps information about the cluster topology
\* Generally not possible to tell failed connection from DCS from the
failure of DCS itself
\* DCS could be inaccessible for some nodes but not others
\* Without DCS it is not possible to figure out if there is another
leader already running
\* To be on the safe side Patroni demotes Postgres
\* https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 129

##### Different site setup & quorom behaviour

Can I have high availability with 2 nodes/2
datacenters?

https://github.com/zalando/patroni/issues/1182
https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 130

##### How does patroni detect failure

TODO: Get more information on the health checks on the node itself

Patroni failovers happening due to issues with Consul health checks https://gitlab.com/gitlab-com/gl-infra/infrastructure/-/issues/7790

##### How to upgrade Patroni?

https://github.com/zalando/patroni/issues/1738

It is as simple as:

Install the new version
Restart patroni
Optionally you can enable maintenance mode by running patronictl pause --wait berore the restart. In this case don't forget to resume it after.


##### Changing max_connections param via edit-config

https://github.com/zalando/patroni/issues/1442 
Pending restart


#### Chaing the replication from async to synchronous



##### Tweaking ttl, loop\_wait, retry\_timeout

* ttl
    * bigger - less “false-positives” due to temporary leader unavailability
    * smaller - faster detection of leader failure
* loop\_wait
    * bigger - less CPU load
    * smaller - faster reaction to external events, i.e. appearance of new members, synchronous replica selection
* retry\_timeout
    * smaller - faster detection of connectivity issues
    * bigger - less false positives due to intermittent connectivity failures.
    https://www.pgcon.org/2019/schedule/attachments/515\_Patroni-training.pdf - slide 57
    
##### Key structure in DCS

When using etcd access the DCS information via 

```ETCDCTL_API=2 etcdctl --endpoint http://HOST ls /service```

Each service is namespaced under: /service/$name
Keys under them are:
* optime -
* history
* members
* initialize
* config
* leader


##### Things to add to the docs

1. Details about the bootstrap  process, what does patroni do?
2. Calling out the document of adding an existing PG node to patroni
3. Auto directory creation for unix\_socket\_directories, stats\_temp\_directory
4. What is the process or reinitialising an existing patroni cluster
5. Common gotcha: Ensuring no other process has been started
6. Common gotcha: Ensure the data\_dir is empty
7. configuring pg\_hba.conf and the precendence
8. Add doc for hba\_file config option