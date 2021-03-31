---
draft: true
title: Debugging
categories:
  - cloud-spanner
tags:
  - performance
  - databases
---

## Transaction related metrics
 - https://cloud.google.com/blog/products/databases/database-transaction-stats-in-spanner
 - https://cloud.google.com/spanner/docs/introspection/transaction-statistics


 ### Useful query 

 ```sql
SELECT
  interval_end,
  avg_total_latency_seconds,
  commit_attempt_count,
  commit_abort_count
FROM SPANNER_SYS.TXN_STATS_TOTAL_10MINUTE;
 ```

 SELECT interval_end, avg_total_latency_seconds, commit_attempt_count, commit_abort_count FROM SPANNER_SYS.TXN_STATS_TOTAL_10MINUTE;

## Lock related metrics

- https://cloud.google.com/blog/topics/developers-practitioners/lock-statistics-diagnose-performance-issues-in-cloud-spanner
- https://cloud.google.com/spanner/docs/introspection/lock-statistics