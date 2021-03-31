---
draft: true
title: dbdeployer quick start
categories:
  - Databases
tags:
  - dbdeployer
---

1. Install

```bash

```
 curl -L -s https://bit.ly/dbdeployer | bash
 dbdeployer 
 dbdeployer init -h
 dbdeployer init --dry-run
 dbdeployer init
 dbdeployer 
 dbdeployer sandboxes
 dbdeployer version
 dbdeployer versions
 dbdeployer delete-binaries
 dbdeployer delete-binaries 8.0.22
 dbdeployer downloads
 dbdeployer downloads list
 dbdeployer downloads 
 dbdeployer downloads get-by-version 5.7.33
 dbdeployer downloads 
 dbdeployer downloads list | grep 5.7.33
 dbdeployer downloads list | grep 5.7
 dbdeployer downloads get-by-version 5.7.31
 dbdeployer 
 dbdeployer versions
 dbdeployer unpack
 ls
 dbdeployer unpack mysql-5.7.31-linux-glibc2.12-x86_64.tar.gz
 dbdeployer version
 dbdeployer versions
 dbdeployer deploy single
 dbdeployer deploy single 5.7.31
 sudo apt-get install ncurses
 sudo apt-cache search libncurses
 sudo apt-get install libncurses5
 dbdeployer deploy single 5.7.31
 dbdeploy usage single
 dbdeployer usage single
 dbdeployer sandboxes list

cd ~/sandboxes/<deployment> 
 ./use 
 ./stop
 ./start