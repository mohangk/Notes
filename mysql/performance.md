#### Performance Shema and Sys schema

1. Provides a way to determine where the database is spending the most time

3 key tables:

* events_waits_summary_global_by_event_name: 
    *  has aggregate data for wait events grouped by the event name
* table_io_waits_summary_by_table: 
    *  related to the wait/io/table/sql/handler in the events_waits_summary_global_by_event_name table. These provide information about the amount of time spent per table split by the time spent for reads, writes, fetch, insert, update, or delete.
* file_summary_by_instance: 
    * shows how much time is spent on actual file I/O and how much data is accessed. very useful for determining where time is spent doing disk I/O and which files have the most data accesses on disk.

2. Enable performance schema
  * https://dev.mysql.com/doc/refman/5.7/en/monitor-alter-table-performance-schema.html

#### references

- https://mysql.wisborg.dk/2018/08/05/what-does-io-latencies-and-bytes-mean-in-the-performance-and-sys-schemas/
- https://aws.amazon.com/blogs/database/best-practices-for-configuring-parameters-for-amazon-rds-for-mysql-part-1-parameters-related-to-performance/
- https://aws.amazon.com/blogs/database/best-practices-for-configuring-parameters-for-amazon-rds-for-mysql-part-2-parameters-related-to-replication/

