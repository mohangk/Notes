#### Dealing with growing databases

* Get the size of the current databases

`SELECT table\_schema "DB Name",ROUND(SUM(data\_length + index\_length) / 1024 / 1024, 1) "DB Size in MB" FROM information\_schema.tables GROUP BY table\_schema;`

* Determine the amount of space used by binary logs
`cd /var/lib/mysql/mysql-bin*`

* Purge some binlogs
`SHOW BINARY LOGS; PURGE BINARY LOGS TO 'mysql-bin-00001.log'`