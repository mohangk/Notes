### MySQL replication

##### master - slave replication architecture

- Master has a "dump thread" that reads the binary log and sends it to the slave IO thread

- IO thread reads the binlog from master and puts it into a relay log

- A SQL thread reads entries from the relay log and writes it to slave disk

##### Types

- binlog file position based replication

- GTID based replication

###### binlog file position

- unique by filename and position - replication coordinates

- Configuration
  
  - On primary
    
    - set: server-id
    
    - enable binary logging: log-bin
    
    - create replication user
  
  - On replica
    
    - mysql> CHANGE MASTER TO -> MASTER_HOST='*source_host_name*', -> MASTER_USER='*replication_user_name*', -> MASTER_PASSWORD='*replication_password*', -> MASTER_LOG_FILE='*recorded_log_file_name*', -> MASTER_LOG_POS=*recorded_log_position*;

###### GTID

- unique identifier produced for every transaction upon commit

- GTID = source_id: transaction_id

- GTIDs are stored in a table named `gtid_executed`, in the `mysql` database

- each transaction can be identified and tracked as it is committed on the originating server and applied by any replicas;  not necessary  to refer to log files or positions within those files when starting a new replica or failing over to a new source, which greatly simplifies these tasks

- Configuration
  
  - Primary and replica
    
    - Enable gtid : gtid_mode=ON, enforce-gtid-consistency=ON

##### 

##### binlog_format

- row
  
  - [Row-based Replication &#8211; Master MySQL](http://www.tocker.ca/2013/09/04/row-based-replication.html)

- stmt - the actual sql statement is written to the log

- mixed

##### replication types

- async - default, commits happen on the master, doesn't care if slave has applied the change

- semy-sync - ensures that at least one slave has received the the commit and stored it into the relay log, only then writes it to master binlog. However slave not necessarily committed it

- group replication / sync - 2PC - ensures that data is committed on at least one slave 


##### debugging replication lag
 - https://making.pusher.com/debugging-mysql-replication-lag/
 - https://aws.amazon.com/premiumsupport/knowledge-center/rds-mysql-high-replica-lag/#Resolution
 - https://scalegrid.io/blog/mysql-tutorial-understanding-the-seconds-behind-master-value/
 
##### enabling parallel replication
https://www.percona.com/blog/2018/10/17/can-mysql-parallel-replication-help-my-slave/
https://www.percona.com/blog/2016/02/10/estimating-potential-for-mysql-5-7-parallel-replication/

##### checking actual replication lag
https://www.percona.com/doc/percona-toolkit/LATEST/pt-heartbeat.html

##### logical backup and restore

- https://severalnines.com/database-blog/mysql-cloud-online-migration-amazon-rds-your-own-server-part2

references

- https://www.percona.com/blog/2017/02/07/overview-of-different-mysql-replication-solutions/
- https://medium.com/@siddontang/dive-into-mysql-replication-protocol-cd14791bcc
- [Setting up MySQL Master-Slave Replication with GTID &#8211; dinfratechsource](https://dinfratechsource.com/2019/05/14/setting-up-mysql-master-slave-replication-with-gtid/) 

##### Differences between primary and replica in replication

Because the binlog is logical - this is possible. However if there are alter tables that refer to indexes that dont exist on the other node it could break the replication

https://stackoverflow.com/questions/4412656/can-you-index-tables-differently-on-master-and-slave-mysql
