---
draft: true
title: Google Cloud Spanner
categories:
  - cloud-spanner
---
- you can optionally define parent-child relationships between tables if you want Cloud Spanner to physically co-locate their rows for efficient retrieval
- If you declare a table to be a child of another table, the primary key column(s) of the parent table must be the prefix of the primary key of the child table. This means if a parent table's primary key is composed of N columns, the primary key of each of its child tables must also be composed of those same N columns, in the same order and starting with the same column.

#Different type of replicas

- read/write replica

Only type of replica if its single region  deployment
Participates in leader election and write commits
Contains complete copy of DB

- read only replica

Doesn’t participate in leader or write commits
Used in multi region deployments

- witness replica

Participate in leader and write commit voting
Doesn’t contain complete copy of DB
Used in multi region deployments

# instance configuration

- permanent for an instance

#node count

- each instance up to 2 TiB of sotorage
- peak read/write depends on instance config & schema design & dataset characteristics
- ~10k qps of reads and ~2kqps of writes

#nodes vs replicas

- Thus, the total number of servers in a Cloud Spanner instance is the number of nodes the instance has multiplied by the number of replicas in the instance.
- For any regional configuration, Cloud Spanner maintains 3 read-write replicas, each within a different Google Cloud Platform availability zone in that region.  Each of this replica also votes.

# multi regional instances

- contains 2 read-write regions, each of which contains two read-write replicas

## tables

### interleaved tables

an interleaved table is a table that you declare to be a child of another table because you want the rows of the child table to be physically stored together with the associated parent row. As mentioned above, the prefix of the primary key of a child table must be the primary key of the parent table.

CREATE TABLE Singers (
  SingerId   INT64 NOT NULL,
  FirstName  STRING(1024),
  LastName   STRING(1024),
  SingerInfo BYTES(MAX),
) PRIMARY KEY (SingerId);

CREATE TABLE Albums (
  SingerId     INT64 NOT NULL,
  AlbumId      INT64 NOT NULL,
  AlbumTitle   STRING(MAX),
) PRIMARY KEY (SingerId, AlbumId),
  INTERLEAVE IN PARENT Singers ON DELETE CASCADE;

- Interleaved rows, cannot delete child  parent row without deleting child rows
- parent and child will be stored in the same data locality , in the same split 
- can further nest an additional child table . requires the entire parent table primary key hierarchy in the primary key of the new child

CREATE TABLE Songs (
  SingerId     INT64 NOT NULL,
  AlbumId      INT64 NOT NULL,
  TrackId      INT64 NOT NULL,
  SongName     STRING(MAX),
) PRIMARY KEY (SingerId, AlbumId, TrackId),
  INTERLEAVE IN PARENT Albums ON DELETE CASCADE;
  
  
  ## spanner query hints
  
  ### FORCE_JOIN_ORDER
  
  ### JOIN_TYPE
  
  - HASH_JOIN
  - LOOP_JOIN
  - APPLY_JOIN
  
  
  ### FORCE_INDEX
  
