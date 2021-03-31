---
draft: true
title: Debugging stalled mysql 
categories:
  - Mysql
---

Run the following periodically (every 5~10 mins)

```sql
SHOW ENGINE INNODB STATUS; SHOW FULL PROCESSLIST 
```

```sql
SELECT current_timestamp(), COUNT(*) FROM information_schema.innodb_lock_waits;
```
```sql
SELECT current_timestamp(); SHOW GLOBAL STATUS WHERE variable_name IN ('Com_insert', 'Com_select', 'Com_update', 'Com_update_multi', 'Com_delete', 'Com_delete_multi', 'Queries', 'Connections'); sleep(10); SHOW GLOBAL STATUS WHERE variable_name in ('Com_insert', 'Com_select', 'Com_update', 'Com_update_multi', 'Com_delete', 'Com_delete_multi', 'Queries', 'Connections'); 
```
This would create a pair of datapoints 10 seconds apart that would need to be delta'd. Running this every few minutes would get us an idea of what kind of queries are being ran on the DB.

```sql
SELECT current_timestamp(); SHOW GLOBAL STATUS WHERE variable_name LIKE 'Threads_running'; 
SELECT current_timestamp(); SET global long_query_time = 0; sleep(300); SET global long_query_time = 2; 
```
This will enable capturing all queries for five minutes to the logs then disable it again. This uses disk space so need to be aware of available space
