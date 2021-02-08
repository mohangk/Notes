---
draft: false
title: BigTable schema basics
categories:
  - Databases
tags:
  - bigtable
---

## Key concepts

### Tables 
- sorted key/value map containing.Consists of columns that are indexed by a row key.
- A Cloud Bigtable table is sharded into blocks of contiguous rows, called tablets, to help balance the workload of queries. (Tablets are similar to HBase regions.) Tablets are stored on Colossus, Google's file system, in SSTable format. An SSTable provides a persistent, ordered immutable map from keys to values, where both keys and values are arbitiary byte strings.

### Column family 
- ways to group common columns together
- Create up to about 100 column families per table. Creating more than 100 column families may cause performance degradation.
- Use different column families for data with different data retention. GC settings are at the column family level
- Columns are grouped by column family and sorted in lexicographic order within the column family. 

### Column qualifiers 
- unique name within a column family. Can contain sparse data. Can be added to on the fly. Store in every row and hence adds to network traffic
- Treat it like data - store something meaningful in it
- Create as many columns as required in a table- can have millions of columns in a table, as long as row doesn't exceed 100MB
- However, avoid using too many columns in a single row - this adds overhead procesing each row. If required put lots of data in a single protobuf and store it in a single column


### Row
- Max of 100MB per row
- Data is automatically  compressed uness it is more then 1MB, then it is not compressed
- Rows are sorted lexicographically by row key, from the lowest to the highest byte string. Row keys are sorted in big-endian byte order (sometimes called network byte order), the binary equivalent of alphabetical order.

### Cell
- max 10MB per cell
- cross section of column and row - can contain timestamped data. Sparse data, empty cells don't take up space
- Each row is essentially a collection of key/value entries, where the key is a combination of the column family, column qualifier and timestamp.

### Row keys
- 4KB or less 
- Most effecient data retrieval
  * Row key
  * Row key prefix
  * Range of rows defined by starting and ending row keys
- Any other type of lookup requires table scans
- Pad integers with zero as the lookup is lexicographical
- In many cases, you should design row keys that start with a common value and end with a granular value. For example, if your row key includes a continent, country, and city, you can create row keys that looks like the following so that they automatically sort first by values with lower cardinality:

### Row key antipatterns 
- Sequential or timestamped key prefixes - sequential writes will be pushed to a single node -creating hotspots
- Hashed values or raw bytes - not human readable making it had to use tools like Key Visualizer to debug hotspots. Also removes the ability to use natural key sorting to do range based lookups
- Frequently updated row key - Try to reduce the need for updates by having keys that store the timestamp so every changes is a new entry

### Datatype support 
- Byte strings only. No native support for more complex types.

### Load balancing traffic

- Each Cloud Bigtable zone is managed by a primary process, which balances workload and data volume within clusters. This process splits busier/larger tablets in half and merges less-accessed/smaller tablets together, redistributing them between nodes as needed. If a certain tablet gets a spike of traffic, Cloud Bigtable splits the tablet in two, then moves one of the new tablets to another node. Cloud Bigtable manages all of the splitting, merging, and rebalancing automatically, saving users the effort of manually administering their tablets. Understanding Cloud Bigtable Performance provides more details about this process.

## Architectures

- Clients -> Front end server -> Node -> Collossus (stores SSTable & shared log)
- Each node has pointers to a set of tablets that are stored on Colossus, data is not stored on the node itself. As a results:
  * Rebalancing tablets from one node to another is very fast, because the actual data is not copied. Cloud Bigtable simply updates the pointers for each node.
  * Recovery from the failure of a Cloud Bigtable node is very fast, because only metadata needs to be migrated to the replacement node.
  * When a Cloud Bigtable node fails, no data is lost.

## Reference
- https://cloud.google.com/bigtable/docs/schema-design