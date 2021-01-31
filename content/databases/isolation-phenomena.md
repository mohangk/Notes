---
draft: false
title: Isolation Phenomena
categories:
  - distributed-systems
tags:
  - databases
---
## Dirty reads 
- Reading changes in t2 from t1 before its committed

## Lost updates
- Changes by t1 are lost due to updates from t2 

## Non repeatable reads 
- Repeated reads of the same record in one transaction results in different values. Happens at "


dirty read
A transaction reads data written by a concurrent uncommitted transaction.

nonrepeatable read
A transaction re-reads data it has previously read and finds that data has been modified by another transaction (that committed since the initial read).

phantom read
A transaction re-executes a query returning a set of rows that satisfy a search condition and finds that the set of rows satisfying the condition has changed due to another recently-committed transaction.

serialization anomaly
The result of successfully committing a group of transactions is inconsistent with all possible orderings of running those transactions one at a time.

## Reference
- https://medium.com/google-cloud/impossible-read-and-write-isolation-phenomena-with-cloud-spanner-8aee06bb6e70
- https://www.postgresql.org/docs/13/transaction-iso.html
- https://dev.mysql.com/doc/refman/5.7/en/innodb-transaction-isolation-levels.html