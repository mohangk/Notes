---
draft: true
title: Mysqlsh
categories:
  - Mysql
---
### mysqlshell

#### Available at

* [MySQL :: Download MySQL Shell](https://dev.mysql.com/downloads/shell/)
* Github - [GitHub - mysql/mysql-shell: MySQL Shell is a new command line scriptable shell for MySQL. It supports JavaScript and Python.](https://github.com/mysql/mysql-shell)

#### Basics

* Establish connection
./mysqlsh user@server/database
* Run SQL inline once started shell
/sql

#### Data dump and restore

###### util.dumpData

* gtid is available in the @.json file in the dump

###### util.loadData

###### Disabling binary logs during data load

Start mysql without bin-log enabled to ensure you dont have to deal with growing binary logs sizes when doing a restore. Sometimes you can't do this within the shell due to the user so you have to do it in the config

```
[mysqld]
skip-log-bin
```

On CloudSQL the binary logs can be disabled like so

gcloud sql instances patch dest-db-57 --no-enable-bin-log

###### Increase the lock\_wait\_timeout

so that parallel loads into the same table dont end up just dying

https://stackoverflow.com/questions/6000336/how-to-debug-lock-wait-timeout-exceeded-on-mysql

\-\- need to figure out if there is a better way to do this with mysqlsh?

###### innodb\_autoinc\_lock\_mode

Set it to 2 if using BINLOG\_FORMAT=ROW
We recommend setting it to 2 when the BINLOG\_FORMAT=ROW. With interleaved, INSERT statements don’t use the table-level AUTO-INC lock and multiple statements can execute at the same time. Setting it to 0 or 1 can cause a huge hit in concurrency for certain workloads.

Interleaved (or 2) is the fastest and most scalable lock mode, but it is not safe if using STATEMENT-based replication or recovery scenarios when SQL statements are replayed from the binary log. Another consideration – which you shouldn’t rely on anyway – is that IDs might not be consecutive with a lock mode of 2. That means you could do three inserts and expect IDs 100,101 and 103, but end up with 100, 102 and 104. For most people, this isn’t a huge deal.
<br>
* https://www.percona.com/blog/2017/07/26/what-is-innodb\_autoinc\_lock\_mode-and-why-should-i-care/
* https://dev.mysql.com/doc/refman/5.7/en/innodb-auto-increment-handling.html

###### innodb\_sort\_buffer\_size

* per-connection setting - default is 1MB 

References

* [MySQL Shell API: Main Page](https://dev.mysql.com/doc/dev/mysqlsh-api-javascript/8.0/)
* [logs - Disable MySQL binary logging with log\_bin variable - Database Administrators Stack Exchange](https://dba.stackexchange.com/questions/72770/disable-mysql-binary-logging-with-log-bin-variable)
* [Hartwork Blog · How to disable MySQL binary logging](https://blog.hartwork.org/posts/how-to-disable-mysql-binary-logging/)
* https://mysqlserverteam.com/mysql-shell-dump-load-part-2-benchmarks/