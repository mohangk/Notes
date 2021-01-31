---
draft: false
title: Debugging Patroni
categories:
  - Patroni
tags:
  - databases
  - postgresql
  - high-availability
---

## Steps to debug Patroni 
  - Increase the log level of Patroni by setting the `PATRONI_LOG_LEVEL="DEBUG"` env var
  - Run patroni in the fore ground to see any errors be displayed in the terminal `patroni /etc/patroni/patroni.yml`