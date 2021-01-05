---
draft: false
title: Durability vs performance trade offs
categories:
  - Mysql
tags:
  - replication
  - performance
---

These are settings that allow you to get more disk performance at the cost of lower durability. Understand the failure modes is key to make a decision on the trade off.

## innodb_flush_log_at_trx_commit

Determines the flushing behaviour of the innodb redo log

- 0: the log buffer is written out to the log file once per second and the flush to disk operation is performed on the log file, but nothing is done at a transaction commit. This will result in lost data if MySQL crashes.

- 1 (the default): the log buffer is written out to the log file at each transaction commit and the flush to disk operation is performed on the log file. 

- 2: the log buffer is written out to the file at each commit, but the flush to disk operation is not performed on it. However, the flushing on the log file takes place once per second also when the value is 2. This will result in loss data if the OS/hardware fails.

There have been cases where setting the value to 2 allows for slaves that are lagging to catchup with the primary as it improves the IO performance.

## sync_binlog

Determine the flushing behaviour of the the binary log. 

- 0: Disables synchronization of the binary log to disk and relies on the operating system to flush the binary log to disk from time to time as it does for any other file. Fastest performance but could leave the, but could end up with data that has been committed (in innodb) that is not in the binlog

- 1: Enables synchronization of the binary log to disk before transactions are committed. Safest setting but lower performance due to increased writes. This could result in transactions in the binlog that havent been committed but this gets resolved as part of the automated recovery process -- they are rolledback. 

- N >1 The binary log is synchronized to disk after N binary log commit groups have been collected. Can result in similar issues as setting it to 0 but can be minimised by setting low values of N. 

### References:
 - https://dba.stackexchange.com/questions/86636/when-is-it-safe-to-disable-innodb-doublewrite-buffering
 - https://www.slideshare.net/JeanFranoisGagn/the-consequences-of-syncbinlog-1/