---
draft: false
title: Performance Schema
categories:
  - Mysql
---

Provides a way to determine where the database is spending the most time

## Key tables

### events_statements_current 
replacement for SHOW FULL PROCESSLIST

### events_statements_summary_by_digest
statistics about classes of queries over time

### events_waits_summary_global_by_event_name: 
has aggregate data for wait events grouped by the event name

### table_io_waits_summary_by_table: 
related to the wait/io/table/sql/handler in the events_waits_summary_global_by_event_name table. These provide information about the amount of time spent per table split by the time spent for reads, writes, fetch, insert, update, or delete.

### file_summary_by_instance: 
shows how much time is spent on actual file I/O and how much data is accessed. very useful for determining where time is spent doing disk I/O and which files have the most data accesses on disk.

### Using performance_schema to monitor DDL related changes

  * https://dev.mysql.com/doc/refman/5.7/en/monitor-alter-table-performance-schema.html

### Enabling performance schema on GCP CloudSQL

Performance schema is not enabled by default on CloudSQL. This needs to be enabled but it [cannot be enabled via the web console and instead needs to be done with gcloud](https://cloud.google.com/sql/docs/mysql/flags#tips). 

When setting the flags remember to set all the existing flags on the instance at the same time like so

```bash
gcloud sql instances patch [INSTANCE_NAME] --database-flags general_log=on,skip_show_database=on,wait_timeout=200000,performance_schema=on
```

### References
- https://orangematter.solarwinds.com/2020/01/12/mysql-query-performance-statistics-in-the-performance-schema/
- https://mysql.wisborg.dk/2018/08/05/what-does-io-latencies-and-bytes-mean-in-the-performance-and-sys-schemas/
- https://aws.amazon.com/blogs/database/best-practices-for-configuring-parameters-for-amazon-rds-for-mysql-part-1-parameters-related-to-performance/
- https://aws.amazon.com/blogs/database/best-practices-for-configuring-parameters-for-amazon-rds-for-mysql-part-2-parameters-related-to-replication/
