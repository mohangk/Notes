---
draft: true
title: Benchmarking
categories:
  - Mysql
---
#### Generating load on mysql

- ./tpcc_start -h rds2.cvsw8xpajw2b.us-east-1.rds.amazonaws.com -d tpcc1000 -u tpcc -p tpccpass -w 20 -r 60 -l 600 -i 10 -c 4

##### Mysqldump and restore

