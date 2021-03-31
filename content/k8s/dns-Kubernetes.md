---
draft: true
title: Dns Kubernetes
categories:
  - K8s
---
### DNS and Kubernetes

#### Object in k8 that has DNS entries

##### Services in k8 are accessible via DNS

##### /etc/resolve.conf

Example:

`nameserver <cluster-ip of kube-dns service> 
search foo.svc.cluster-foobar.internal svc.cluster-foobar.internal cluster-foobar.internal 
options ndots:5`



Injection of DNS lookups happen if records has less then 5 dots

?? dnsPolicy: ClusterFirst
