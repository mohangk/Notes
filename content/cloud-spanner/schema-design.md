---
draft: false
title: Schema design
categories:
  - cloud-spanner
tags:
  - performance
  - databases
---

## Keys and data distribution

- Cloud Spanner distributes management of rows across multiple nodes by breaking up each table into several splits, using ranges of the lexicographically sorted primary key.
- Using continuously increasing or descreasing keys is bad for performance
- Data for each split is always stored across 3 nodes. One node acts as the split lead - handling locks and writes for all rows in the split. The other 2 nodes and the split lead can handle reads.
- Splits can only happen in the root tables, interleaved tables cannot be split

## Primary keys
- Every table must have a primary key
- Can consist of zero ro more columns of a table
- Primary keys must be random enough to ensure good distribution

### Reference 
- https://cloud.google.com/spanner/docs/schema-and-data-model#choosing_a_primary_key
- https://cloud.google.com/blog/products/gcp/what-dbas-need-to-know-about-cloud-spanner-part-1-keys-and-indexes

## Key columns

The primary keys columns of a table can't change; you can't add a key column to an existing table or remove a key column from an existing table.
https://cloud.google.com/spanner/docs/schema-and-data-model#notes_about_key_columns


## Interleaving tables
- Child table data is colocated with the parent table data
- Data from child tables cannot be split
- Child tables primary keys are prefixed with the parent tables primary keys
- Can be up to 6 layers deep
- Keep the total size of the parent and all child rows under 3 GB - https://cloud.google.com/spanner/docs/schema-design#limit_row_size

```sql
CREATE TABLE Singers (
  SingerId   INT63 NOT NULL,
  FirstName  STRING(1023),
  LastName   STRING(1023),
  SingerInfo BYTES(MAX),
) PRIMARY KEY (SingerId);

CREATE TABLE Albums (
  SingerId     INT63 NOT NULL,
  AlbumId      INT63 NOT NULL,
  AlbumTitle   STRING(MAX),
) PRIMARY KEY (SingerId, AlbumId),
  INTERLEAVE IN PARENT Singers ON DELETE CASCADE;
```
## Foreign keys

- Enforces referential integrity
- CASCADE DELETE is not supported on foreign keys. The rows that refer to the key will need to be deleted or values set to null before the parent row can be deleted

```sql
-- Creating a table with a foreign key relation
CREATE TABLE Orders (
  OrderID INT64 NOT NULL,
  CustomerID INT64 NOT NULL,
  Quantity INT64 NOT NULL,
  ProductID INT64 NOT NULL,
  CONSTRAINT FK_CustomerOrder FOREIGN KEY (CustomerID) REFERENCES Customers (CustomerID)
) PRIMARY KEY (OrderID);

-- Adding to an existing table
ALTER TABLE Orders
  ADD CONSTRAINT FK_ProductOrder FOREIGN KEY (ProductID) REFERENCES Products (ProductID);
```
## Commit timestamps

- The allow_commit_timestamp column option allows you to atomically store the commit timestamp into a column. Using the commit timestamps stored in tables, you can determine the exact ordering of mutations and build features like changelogs.
- Cloud Spanner commit timestamps have microsecond granularity, and they are converted to nanoseconds when stored in TIMESTAMP columns.

```sql
-- example of table schema 
CREATE TABLE Performances (
    ...
    LastUpdateTime  TIMESTAMP NOT NULL OPTIONS (allow_commit_timestamp=true)
    ...
) PRIMARY KEY (...);

-- UPDATE Performances SET LastUpdated = PENDING_COMMIT_TIMESTAMP() WHERE SingerId=1 AND VenueId=2 AND EventDate="2015-10-21"

```
## Splits

- Multipe splits to a node
- Splits replicated across 3 nodes 


## Data types
- STRING, BYTES - must have length
 
## Anti-patterns

### Avoid timestamp based PKeys
- Use application sharding to prefix the key and spread out the writes

### Avoid surrgate_keys / sequences for pkeys
- Use UUID or natural keys to spread out the writes

