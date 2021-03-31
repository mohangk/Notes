---
draft: true
title: Howto
categories:
  - Go-Ycsb
---
##### Populate the YCSB database

./bin/go-ycsb load mysql -p mysql.host=127.0.0.1 -p mysql.port=3306 -p mysq.user=root -p mysql.db=test -p recordcount=1000 -p threadcount=2

##### run workload a

./go-ycsb run mysql -P ../workloads/workloada -p mysql.host=10.128.0.8 -p mysql.user=application -p mysql.db=test -p mysql.password=password -p recordcount=1000 -p threadcount=2

References:

- https://medium.com/@siddontang/use-go-ycsb-to-benchmark-different-databases-8850f6edb3a7
