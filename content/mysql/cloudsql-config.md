---
draft: true
title: Cloudsql Config
categories:
  - Mysql
---
* enabling/disabling bin\_log
    * gcloud sql instances patch dest-db-57 --no-enable-bin-log

* always enable performance schema
   * gcloud sql instances patch <instance> --database-flags performance_schema=on