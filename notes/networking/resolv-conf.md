##### /etc/resolve.conf

##### Background

It is the configuration file for the c-lib (typically glibc, could be musl) dns resolver 

Example:

  search foo.svc.cluster-foobar.internal svc.cluster-foobar.internal
  nameserver 1.1.1.1
  nameserver 8.8.8.8
  options ndots:5

##### Important config options in the file

1. Nameserver - list of nameservers to query, otherwise query the local name server

2. search 
