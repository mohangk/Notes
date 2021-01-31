---
draft: false
title: Edit Config
categories:
  - Patroni
tags:
  - databases
  - postgresql
  - high-availability
---

## edit-config
- Configuration values that are stored in the DCS can be modified as part of the `patronictl -c <conf file> edit-config` command
- Some configuration values require a restart of PostgreSQL - this is visible via the output of `patronictl -c <conf file> list` command
- https://blog.dbi-services.com/patroni-operations-changing-parameters/