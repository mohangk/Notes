---
draft: true
title: Backups
categories:
  - Postgresql
---
##### disk snapshot based backups

* Before proceeding with the upgrade itself, Patroni had to be signaled to avoid any spurious leader election, take a consistent backup through GCP Snapshots (using corresponding [low-level backup API](https://www.cybertec-postgresql.com/en/exclusive-backup-deprecated-what-now/?gclid=CjwKCAjwltH3BRB6EiwAhj0IUBjiSxBdmS11SUpITLCmk-oPkBa7udOWyA6bK6hig8neaiJc8n1WexoCq8UQAvD_BwE)) and apply the new settings via Chef run.
* The ongres guys used disk snapshots when they were making backups for the gitlab upgrade here - https://about.gitlab.com/blog/2020/09/11/gitlab-pg-upgrade/ look for "Pre-upgrade steps of the PostgreSQL" BUT I think they stopped PG by then - relevant ansible playbook - https://gitlab.com/gitlab-com/gl-infra/db-migration/-/blob/master/pg-upgrade/playbooks/upgrade.yml#L147

[Exclusive backup method is deprecated - what now? - Cybertec](https://www.cybertec-postgresql.com/en/exclusive-backup-deprecated-what-now/)

#### pg_basebackup

- Makes a binary backup of the database from primary to replica using the same protocol as the replication. Can also be done from a different replica (chained replica)
- Requires SUPERUSER or REPLICATION privilegess
- must be set to explicitly permit replication connections
- need to be set high enough to allow for both backup and other time of WAL streaming
- When backup happens, its important to have all the WAL files that were generated from the start of the backup till the end of the backup to be able to rebuild a consistent view of the data. To do this base backup history process createa backup history file in the WAL archite area. It is named after the first WAL segment needed for the backup. For example, if the starting WAL file is 0000000100001234000055CD the backup history file will be named something like 0000000100001234000055CD.007C9330.backup. (The second part of the file name stands for an exact position within the WAL file, and can ordinarily be ignored.) -- question: Does this only happen if archive\_command is set?

##### Non-exclusize low level backup

1. Requires WAL archiving
1. `SELECT pg_start_backup('label', false, false)`
a. Does a `CHECKPOINT` to ensure everything is flushed out to disk
b. Set the second param to true to force a CHECKPOINT immediately
c. 3rd option being set to `false` makes the backup non-exclusive
d. Connection running the pg_start_backup needs to be left running, otherwise stops
2. `SELECT pg_stop_backup(false, true)`
a. Terminates backup mode.
b. The pg_stop_backup will return one row with three values. The second of these fields should be written to a file named backup_label in the root directory of the backup. The third field should be written to a file named tablespace_map unless the field is empty. These files are vital to the backup working, and must be written without modification.

<br>
References:
* https://www.postgresql.org/docs/13/continuous-archiving.html
* https://about.gitlab.com/blog/2020/09/11/gitlab-pg-upgrade/
* [https://www.2ndquadrant.com/en/blog/what-does-pg\_start\_backup-do/](https://www.2ndquadrant.com/en/blog/what-does-pg_start_backup-do/)
* https://www.postgresql.org/message-id/166191299.1443519.1516368751643%40mail.virgilio.it - discussion on using cloud disk snapshots for backup