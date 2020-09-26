### mysqlshell

#### Available at

- [MySQL :: Download MySQL Shell](https://dev.mysql.com/downloads/shell/)

- Github - [GitHub - mysql/mysql-shell: MySQL Shell is a new command line scriptable shell for MySQL. It supports JavaScript and Python.](https://github.com/mysql/mysql-shell)



#### Basics

- Establish connection
  
  ./mysqlsh user@server/database

- Run SQL inline once started shell
  
  /sql <TYPE YOUR SQL here>

#### Data dump and restore

###### util.dumpData

- gtid is available in the @.json file in the dump

###### util.loadData



Disabling binary logs during data load

Start mysql without bin-log enabled to ensure you dont have to deal with growing binary logs sizes when doing a restore. Sometimes you can't do this within the shell due to the user so you have to do it in the config

```
[mysqld]
skip-log-bin
```

References

- [MySQL Shell API: Main Page](https://dev.mysql.com/doc/dev/mysqlsh-api-javascript/8.0/)
- [logs - Disable MySQL binary logging with log_bin variable - Database Administrators Stack Exchange](https://dba.stackexchange.com/questions/72770/disable-mysql-binary-logging-with-log-bin-variable)
- [Hartwork Blog · How to disable MySQL binary logging](https://blog.hartwork.org/posts/how-to-disable-mysql-binary-logging/)
