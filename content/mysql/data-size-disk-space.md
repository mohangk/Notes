#### Dealing with growing databases

* Get the size of the current databases

`SELECT table_schema "DB Name",ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" FROM information_schema.tables GROUP BY table_schema;`

* Get the size of the tables in the database

SELECT   TABLE_NAME AS `Table`,   ROUND((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS `Size (MB)` FROM   information_schema.TABLES WHERE   TABLE_SCHEMA = "imdb" ORDER BY   (DATA_LENGTH + INDEX_LENGTH) DESC;


* Determine the amount of space used by binary logs
`cd /var/lib/mysql/mysql-bin*`

* Purge some binlogs
`SHOW BINARY LOGS; PURGE BINARY LOGS TO 'mysql-bin-00001.log'`

* Purge all binlogs
`RESET MASTER`