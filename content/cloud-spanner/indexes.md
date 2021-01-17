---
draft: false
title: Indexes
categories:
  - cloud-spanner
tags:
  - databases
---
## Indexes

Because indexes are implemented as tables in Cloud Spanner, you’ll encounter the same issues with the indexed columns as you did with the table keys: An index on a column with poorly distributed values (such as a timestamp) will lead to the creation of a hotspot, even if the underlying table is using well-distributed keys.


### Example of index that will hotspot and how to address it
```sql
CREATE TABLE Events (
      UserId String(MAX),
      Timestamp TIMESTAMP,
      EventData)
PRIMARY KEY (UserId, Timestamp DESC);
CREATE INDEX EventsByTimestamp ON Events (Timestamp DESC);
```

- Add a synthetic shard key `TimestampShardId = CRC32(Timestamp) % 100`

```sql
CREATE TABLE Events (
     UserId String(MAX),
     Timestamp TIMESTAMP,
     TimestampShardId INT64, 
     EventData)
PRIMARY KEY (UserId, Timestamp DESC);
CREATE INDEX EventsByTimestamp ON Events (TimestampShardId,Timestamp);
```

Queries have to be modified now to factor in the shard IDs otherwise index won't be used and it will be a table scan

```sql
Select * from Events@{FORCE_INDEX=EventsByTimestamp}
WHERE
   TimestampShardId BETWEEN 0 AND 99
   AND Timestamp > @lower_bound
   AND Timestamp < @upper_bound;
```


### NULL_FILTERED index

- Ignore NULL columns from an index
```sql
CREATE TABLE Employees (
      CompanyUUID INT64,
      EmployeeUUID INT64,
      FullName STRING(MAX)
            ...
) PRIMARY KEY (CompanyUUID,EmployeeUUID)

CREATE INDEX EmployeesById 
      ON Employees (EmployeeUUID) 
      STORING (FullName);
```
### Storing INDEX

- A form of Covering Index allowing for index only lookups
```
CREATE INDEX EmployeesById 
      ON Employees (EmployeeUUID) 
      STORING (FullName);
```

### Force INDEX use

```
Select * 
from  Employees@{FORCE_INDEX=EmployeesById}
Where EmployeeUUID=xxx;
```

### Reference
- https://cloud.google.com/blog/products/gcp/what-dbas-need-to-know-about-cloud-spanner-part-1-keys-and-indexes