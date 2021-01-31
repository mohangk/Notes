---
draft: false
title: Read write locks 
categories:
  - databases
tags:
  - transactions
---


## Simple Locking: 
- Two main types of locks can be requested:
  * Shared lock: Read lock i.e. Any other TX can read but not write
  * Exclusive lock: Write lock i.e. No other TX can read or write

## Multiple Locking: 
- Two Phase Locking (2PL) is a concurrency control method that guarantees serializability.
  * A Growing/Expanding/First Phase: locks are acquired and no locks are released.
  * A Shrinking/Contracting/Second Phase: locks are released and no locks are acquired.

## Reference
https://stackoverflow.com/questions/7713049/read-locks-and-write-locks
