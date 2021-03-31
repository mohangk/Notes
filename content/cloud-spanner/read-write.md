---
draft: false
title: Read / write operation
categories:
  - cloud-spanner
tags:
  - databases
---
## Singular reads
- Read outside of the context of a transaction
- Strong read - gurantee dto see all data that has been committed up to that point
- Stale read - in the past 

## Parallel reads 
- If a read query has a "Distributed Union" as part of a query plan, the read can be done in parallel
- https://cloud.google.com/spanner/docs/reads#read_data_in_parallel
- 
## Read write transactions
- one or more writes that need to be committed atomically or you might do one or more writes based on reads

Read-write transactions are used for transactions that insert, update, or delete data; this includes transactions that perform reads followed by a write. They are still highly scalable, but read-write transactions introduce locking and must be orchestrated by Paxos leaders. Note that locking is transparent to Spanner clients.

### Reference 
- https://cloud.google.com/spanner/docs/transactions#read-write_transactions

Any read-write or read-only transaction that should see the effects of mutations waits until the mutations are applied before attempting to read the data. For read-write transactions, this is enforced because the transaction must take a read lock. For read-only transactions, this is enforced by comparing the read's timestamp with that of the latest applied data.

## Read only transactions
Spanner provides both read-only transactions and read-write transactions. The former are the preferred transaction-type for operations (including SQL SELECT statements) that do not mutate your data. Read-only transactions still provide strong consistency and operate, by-default, on the latest copy of your data. But they are able to run without the need for any form of locking internally, which makes them faster and more scalable. 
## Writes 

- Write performance is determined by whether the transaction is a multi split or single split write

### Single split write
- Only requires coordination between the replicas of the splits, considered a local operation. 

### Multi split writes
- If multiple splits are involved, an extra layer of coordination (using the standard two-phase commit algorithm) is necessary.

### Blind writes 
- Allow for multiple transactions writing the same data to proceed in parallel. This can happen if the transaction is only writing to the data without first reading it.


### Handling deadlocks

- Spanner uses a standard "wound-wait" deadlock prevention algorithm to ensure that transactions make progress. In particular, a "younger" transaction will wait for a lock held by an "older" transaction, but an "older" transaction will "wound" (abort) a younger transaction holding a lock requested by the older transaction. Therefore we never have deadlock cycles of lock waiters.

### fsyncs

- Spanner doesn't need to wait for fsyncs. Storage has batter backed clocks.

## Reads 
## Strong read consistency

- When writing the Leader waits until it can be sure that the transaction's timestamp has passed in real time; this typically requires a few milliseconds so that we can wait out any uncertainty in the TrueTime timestamp. This is what ensures strong consistency—once a client has learned the outcome of a transaction, it is guaranteed that all other readers will see the transaction's effects. This "commit wait" typically overlaps with the replica communication in the step above, so its actual latency cost is minimal. More details are discussed in this paper.

-  "Strong" reads of the latest data may spend some latency verifying whether a replica is up-to-date enough; but a read that can be satisfied with stale data does not pay this latency. 


## Locking

### ReaderShared lock 
- A lock that allows other reads to still access the data until the transaction is ready to commit. Acquired when a read-write transaction reads data. 

### Writer shared lock
- Lock is acquired when a read-write transaction tries to commit a write.
- Spanner has the ability to do shared-write locks (writer shard lock)if the transactions are doing "blind writes".

### Exclusive
 - an exclusive lock is acquired when a read-write transaction, which has already acquired a ReaderShared lock, tries to write data after the completion of read and upgrades to a WritesShared lock. A special case of a transaction holding both the ReaderShared lock and WriterShared lock at the same time. No other transaction can acquire any lock on the same cell.

### Reference

- https://cloud.google.com/spanner/docs/introspection/lock-statistics#explain-lock-modes

## Transactions 

### read write transactions

### read only transactions
- One difference is that if you read data, and later commit the read-write transaction without any writes, there is no guarantee that the data did not change in the database after the read and before the commit. If you want to know whether data has changed since you read it last, the best approach is to read it again (either in a read-write transaction, or using a strong read.) 


### Reference
- https://cloud.google.com/spanner/docs/whitepapers/life-of-reads-and-writes
- https://cloud.google.com/spanner/docs/transactions