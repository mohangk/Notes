---
draft: true
title: Pitr
categories:
  - Postgresql
---

In its place, one of two "signal" files may be placed in the data directory:

“standby.signal” – indicates the server should start up as a hot standby
“recovery.signal” – indicates the server should start up in targeted recovery mode
If both files are present, "standby.signal" takes precedence. The files do not need to contain any data; any data present will be ignored.

If a standby is promoted, "standby.signal" is removed entirely (and not renamed as was the case with "recovery.conf", which became "recovery.done"). Similarly, if point-in-time recovery is taking place, "recovery.signal" will be removed once the recovery target is reached (unless "recovery_target_action" is set to "shutdown").

Note that as with "recovery.conf", the absence of these files from the directory will cause the PostgreSQL instance to start up straight away as a primary. This will happen regardless of whether "postgresql.conf" contains replication configuration.

Only one parameter from the “recovery_target” family may be specified
If more than one of “recovery_target”, “recovery_target_lsn”, “recovery_target_name”, “recovery_target_time” or “recovery_target_xid” is present in the PostgreSQL configuration, the instance will emit a FATAL error at startup:


Reference:
