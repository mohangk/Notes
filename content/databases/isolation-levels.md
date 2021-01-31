---
draft: false
title: Isolation levels
categories:
  - Databases
tags:
  - databases
---

## Isolation levels

### serialisable isolation
  
  - as thought transactions  are executed one by one instead of concurrenlty

### Snapshot isolation
  
  - each transaction sees the fix commted database state for the entire duration of the transaction. writes performed in a transaction appear to apply automatically at  commit time, and can only commit if similar items were not modified after the transaction started - dirty writes

### Repeatable reads
  
-  this level forbids dirty reads (non committed data) and non repeatable reads (executing the same query twice should return the same values) and allows phantom reads (new rows are visible).

### Read comitted

- Read Committed is the default isolation level in PostgreSQL. When a transaction uses this isolation level, a SELECT query (without a FOR UPDATE/SHARE clause) sees only data committed before the query began; it never sees either uncommitted data or changes committed during query execution by concurrent transactions. In effect, a SELECT query sees a snapshot of the database as of the instant the query begins to run. 
- However, SELECT does see the effects of previous updates executed within its own transaction, even though they are not yet committed.
- Also note that two successive SELECT commands can see different data, even though they are within a single transaction, if other transactions commit changes after the first SELECT starts and before the second SELECT starts.
- Because Read Committed mode starts each command with a new snapshot that includes all transactions committed up to that instant, subsequent commands in the same transaction will see the effects of the committed concurrent transaction in any case. The point at issue above is whether or not a single command sees an absolutely consistent view of the database.


### Read uncommitted

## Default isolation levels 
- Postgresql - read committed
- Oracle - read comitted
- Mysql - repeatable read for SELECTS, read committed for DML

### References
- https://www.postgresql.org/docs/13/transaction-iso.html#XACT-READ-COMMITTED
- https://blog.pythian.com/understanding-mysql-isolation-levels-repeatable-read/
- https://dev.mysql.com/doc/refman/5.7/en/innodb-transaction-isolation-levels.html



## Harmful workloads for databases

- [GitHub - lesovsky/noisia: Harmful workload generator for PostgreSQL](https://github.com/lesovsky/noisia)
