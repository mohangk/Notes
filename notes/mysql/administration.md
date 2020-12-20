https://fromdual.com/mysql-performance-schema-hints#unused-indexes

https://mysql.wisborg.dk/2018/08/05/what-does-io-latencies-and-bytes-mean-in-the-performance-and-sys-schemas/

###### unused index and their size
```sql
SELECT t.object_name, 
    t.index_name, 
    ROUND(i.stat_value * @@innodb_page_size / 1024 / 1024, 2) size_in_mb
FROM 
    performance_schema.table_io_waits_summary_by_index_usage as t 
JOIN 
    mysql.innodb_index_stats as i 
ON 
    t.object_schema = i.database_name 
    AND t.object_name = i.table_name 
    AND t.index_name = i.index_name 
    AND i.stat_name = 'size'
WHERE 
    t.index_name IS NOT NULL  
    AND t.index_name != 'PRIMARY' 
    AND t.count_star = 0 
    AND t.object_schema = 'dbname'  
ORDER BY t.object_schema, t.object_name;
```
##### temporary disabling indexes

MySQL 8 supports invisible indexes, but 5.7 does not.  The only thing you can do on 5.7 is add an IGNORE_INDEX hint to queries that would use the index, so that the optimizer doesn't pick it.