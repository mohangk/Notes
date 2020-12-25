## Different tools


The AWS Benchmark mentioned above is based on pgbench and sysbench.

HammerDB, also mentioned earlier, is discussed in a recent post on pgsql-hackers list.

TPC-C tests based on oltpbench as alluded in this other pgsql-hackers discussion.

benchmarksql is yet another TPC-C test that was used to validate the changes to B-Tree page splits.

* pg_ycsb - https://github.com/postgrespro/pg_ycsb - ycsb implemented in pgbench


pgbench-tools as the name suggests, is based on pgbench and while not having received any updates since 2016, it is the product of Greg Smith, the author of PostgreSQL High Performance books.

join order benchmark is a benchmark that will test the query optimizer.

pgreplay which I came across while reading the Command Prompt blog is as close as it can get to benchmarking a real life scenario.



Reference:

https://severalnines.com/database-blog/benchmarking-managed-postgresql-cloud-solutions-part-one-amazon-aurora


## pgbench

* pgbench -i - initailise the tables that are used by pgbench
* pgbench -i -Uxx -hxx dbname - provide a database to be used by pgbench

### Initialising

 * Initialising to get the right sized database https://www.cybertec-postgresql.com/en/a-formula-to-calculate-pgbench-scaling-factor-for-target-db-size/

pgbench -i -s 100 - 10 mil rows ~ 20secs to populate, ~ 1.5GB data

### Custom benchmarks

https://www.cybertec-postgresql.com/en/insert-only-data-modeling-with-postgresql/
https://satob.hatenablog.com/entry/2020/05/01/035727




