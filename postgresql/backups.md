##### backups

Before proceeding with the upgrade itself, Patroni had to be signaled to avoid any spurious leader election, take a consistent backup through GCP Snapshots (using corresponding [low-level backup API](https://www.cybertec-postgresql.com/en/exclusive-backup-deprecated-what-now/?gclid=CjwKCAjwltH3BRB6EiwAhj0IUBjiSxBdmS11SUpITLCmk-oPkBa7udOWyA6bK6hig8neaiJc8n1WexoCq8UQAvD_BwE)) and apply the new settings via Chef run.

[Exclusive backup method is deprecated - what now? - Cybertec](https://www.cybertec-postgresql.com/en/exclusive-backup-deprecated-what-now/)

References:

- https://about.gitlab.com/blog/2020/09/11/gitlab-pg-upgrade/
