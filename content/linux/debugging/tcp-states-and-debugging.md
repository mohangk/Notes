---
draft: true
title: Tcp States And Debugging
categories:
  - Linux
---
1. TCP handshake:
- client -> server: syn ( client: syn-sent)

- when server receives it server is in syn-rec. Then server sends syn-ack

- ack

Socket states

- established

- syn-sent,

- syn-recv,

- fin-wait-1,

- fin-wait-2,

- time-wait,

- closed,

- close-wait,

- last-ack,

- listening

- closing.
  
  ```
         all - for all the states       connected - all the states except for listening and closed       synchronized - all the connected states except for syn-sent       bucket - states, which are maintained as minisockets, i.e.  time-wait and syn-recv       big - opposite to bucket
  ```

- 

- Read / write socket buffers
  
  1. [root@host ~]# sysctl -a | egrep "tcp_(r|w)mem"  
     net.ipv4.tcp_rmem = 4096 1048576 4194304  
     net.ipv4.tcp_wmem = 4096 1048576 4194304

### Tools to debug connection statuses

- ss -tap

Misc:

1. [https://unix.stackexchange.com/questions/258711/alternative-to-netstat-s](https://unix.stackexchange.com/questions/258711/alternative-to-netstat-s)

2. [https://wiki.archlinux.org/index.php/Network_configuration#Investigate_sockets](https://wiki.archlinux.org/index.php/Network_configuration#Investigate_sockets)
