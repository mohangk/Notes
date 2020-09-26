#### Databases

##### Isolation levels

- serialisable isolation
  
  - as thought transactions  are executed one by one instead of concurrenlty

- Snapshot isolation
  
  - each transaction sees the fix commted database state for the entire duration of the transaction. writes performed in a transaction appear to apply automatically at  commit time, and can only commit if similar items were not modified after the transaction started 

- dirty writes

- repeatable reads
  
  - dirty reads

##### Harmful workloads for databases

- [GitHub - lesovsky/noisia: Harmful workload generator for PostgreSQL](https://github.com/lesovsky/noisia)
