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

