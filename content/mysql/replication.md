---
draft: false
title: Mysql replication
categories:
  - mysql

---

* Master has a "dump thread" that reads the binary log and sends it to the slave IO thread
* IO thread reads the binlog from master and puts it into a relay log
* A SQL thread reads entries from the relay log and writes it to slave disk
* By default both the IO and SQL thread is single threaded. In mysql 5.7 onwards there is the ability to enable parallel replication.


## Types

* binlog file position based replication
* GTID based replication

### binlog file position

* unique by filename and position - replication coordinates
* Configuration
    * On primary
        * set: server-id
        * enable binary logging: log-bin
        * create replication user
    * On replica
        * mysql> CHANGE MASTER TO -> MASTER\_HOST='*source\_host\_name*', -> MASTER\_USER='*replication\_user\_name*', -> MASTER\_PASSWORD='*replication\_password*', -> MASTER\_LOG\_FILE='*recorded\_log\_file\_name*', -> MASTER\_LOG\_POS=*recorded\_log\_position*;

### GTID

* unique identifier produced for every transaction upon commit
* GTID = source\_id: transaction\_id
* GTIDs are stored in a table named `gtid_executed`, in the `mysql` database
* each transaction can be identified and tracked as it is committed on the originating server and applied by any replicas; not necessary to refer to log files or positions within those files when starting a new replica or failing over to a new source, which greatly simplifies these tasks
* Configuration
    * Primary and replica
        * Enable gtid : gtid\_mode=ON, enforce-gtid-consistency=ON

### binlog\_format

* row
    * [Row-based Replication – Master MySQL](http://www.tocker.ca/2013/09/04/row-based-replication.html)
* stmt - the actual sql statement is written to the log
* mixed

##### replication types

* async - default, commits happen on the master, doesn't care if slave has applied the change
* semi-sync - ensures that at least one slave has received the the commit and stored it into the relay log, only then writes it to master binlog. However slave not necessarily committed it
* group replication / sync - 2PC - ensures that data is committed on at least one slave

##### debugging replication lag

* Replication lag can happen either due to a delay on the IO Thread (delays in transferring the data from the primary to the replica) or SQL Thread (due to delays in processing the backlog of the binlogs)
* To determine which is the root cause
    1.  Run "SHOW MASTER STATUS" on the primary node. Take note of the file and the position
    2.  On the replica run "SHOW SLAVE STATUS". Take note of the following fields:
        - Master_Log_File and the Read_master_log_Pos - shows the copied over binlogs
        - Relay_Master_Log_file - indicates at which point which the SQL Thread has caught up with the binlog
    3. If the gap is between the maste_log_file on the primary and replica, the lag is due to the IO Tread. If the gap is between the relay_master_log_file and the master_log_file on the replica, then the delay is due a lag in the SQL Thread

### References
* [How Booking.com Avoids and Deals with MySQL/MariaDB Replication Lag](https://www.youtube.com/watch?v=9caro2QNcww)
* https://making.pusher.com/debugging-mysql-replication-lag/
* https://aws.amazon.com/premiumsupport/knowledge-center/rds-mysql-high-replica-lag/#Resolution
* https://scalegrid.io/blog/mysql-tutorial-understanding-the-seconds-behind-master-value/

## Enabling parallel replication

### Slave parallel type
  - DATABASE: Parallelism only among multiple databases, not too useful
  - LOGICAL_CLOCK: Parallism within the same database. In 5.7.4 it uses intervals to determine which are the transactions that can be executed together. The larger the interval, the more transactions that can be executed together. In 8.0 the ability to identify the intervals have been approved.

#### Reference
* https://jfg-mysql.blogspot.com/2017/02/metric-for-tuning-parallel-replication-mysql-5-7.html

### How to determine the performance of parallel replication?

In the performance_schema tables, you can get timing of the coordinator and workers on the replica via the replication_applier_status_by_coordinator, replication_applier_status_by_worker tables

#### Reference
- https://mysql.wisborg.dk/2018/10/05/replication-monitoring-with-the-performance-schema/





### Optimizing parallel replication
- Slowling down the master to batch commit transactions together `binlog_group_commit_sync_delay`, `binlog_group_commit_sync_no_delay_count`

### References
- MySQL Parallel Replication (LOGICAL_CLOCK): all the 5.7 and some 8.0 details
  - https://www.slideshare.net/JeanFranoisGagn/mysql-parallel-replication-logicalclock-all-the-57-and-some-of-the-80-details
  - https://www.youtube.com/watch?v=cPsULwEnCio
- https://www.percona.com/blog/2018/10/17/can-mysql-parallel-replication-help-my-slave/
- https://www.percona.com/blog/2016/02/10/estimating-potential-for-mysql-5-7-parallel-replication/



### Binlog servers 
Server that can forward binary logs along. Removes the need for intermediary masters solely for the purpose of forwarding bin logs between distributed DCs. Also ensures that the binary logs forwarded are exactly the same as received from the master.

- https://github.com/google/mysql-ripple

##### checking actual replication lag

https://www.percona.com/doc/percona-toolkit/LATEST/pt-heartbeat.html

##### logical backup and restore

* https://severalnines.com/database-blog/mysql-cloud-online-migration-amazon-rds-your-own-server-part2

references

* https://www.percona.com/blog/2017/02/07/overview-of-different-mysql-replication-solutions/
* https://medium.com/@siddontang/dive-into-mysql-replication-protocol-cd14791bcc
* [Setting up MySQL Master-Slave Replication with GTID – dinfratechsource](https://dinfratechsource.com/2019/05/14/setting-up-mysql-master-slave-replication-with-gtid/)

##### Differences between primary and replica in replication

Because the binlog is logical - this is possible. However if there are alter tables that refer to indexes that dont exist on the other node it could break the replication

https://stackoverflow.com/questions/4412656/can-you-index-tables-differently-on-master-and-slave-mysql
