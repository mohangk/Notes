##### disk snapshot based backups

- Before proceeding with the upgrade itself, Patroni had to be signaled to avoid any spurious leader election, take a consistent backup through GCP Snapshots (using corresponding [low-level backup API](https://www.cybertec-postgresql.com/en/exclusive-backup-deprecated-what-now/?gclid=CjwKCAjwltH3BRB6EiwAhj0IUBjiSxBdmS11SUpITLCmk-oPkBa7udOWyA6bK6hig8neaiJc8n1WexoCq8UQAvD_BwE)) and apply the new settings via Chef run.

- The ongres guys used disk snapshots when they were making backups for the gitlab upgrade here - https://about.gitlab.com/blog/2020/09/11/gitlab-pg-upgrade/ look for "Pre-upgrade steps of the PostgreSQL" BUT I think they stopped PG by then - relevant ansible playbook - https://gitlab.com/gitlab-com/gl-infra/db-migration/-/blob/master/pg-upgrade/playbooks/upgrade.yml#L147

[Exclusive backup method is deprecated - what now? - Cybertec](https://www.cybertec-postgresql.com/en/exclusive-backup-deprecated-what-now/)

References:

* https://about.gitlab.com/blog/2020/09/11/gitlab-pg-upgrade/
* [https://www.2ndquadrant.com/en/blog/what-does-pg\_start\_backup-do/](https://www.2ndquadrant.com/en/blog/what-does-pg_start_backup-do/)
* https://www.postgresql.org/message-id/166191299.1443519.1516368751643%40mail.virgilio.it - discussion on using cloud disk snapshots for backup