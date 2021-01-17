---
draft: true
title: Isolation levels 
categories:
  - cloud-spanner
---
- t1 starts before t2 
- t1 commits before t2 commits
- at the point of t1 commiting changes from t1 are immediately available to t2. This is not snapshot isolation
- data that is committed in a transaction t1 that started before t2 is available 
- https://cloud.google.com/spanner/docs/true-time-external-consistency