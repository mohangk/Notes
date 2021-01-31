---
draft: true
title: Systemd
categories:
  - Postgresql
---
##### Gotchas

- systemd automatically unmount filesystem
    - https://bugzilla.redhat.com/show_bug.cgi?id=1494014
    - Workaround: systemctl daemon-reload