---
draft: false
title: Load balancers
categories:
  - Gcp
tags:
  - load-balancers
---
## High level architecture

```ascii
        (backends)
VMs --> Instance group --> backend service --> fowarding rules
```
## Backends
### Different type of backends
  - Instance groups (managed or unmanaged) containing virtual machine (VM) instances. 
  - Zonal NEG
  - Serverless NEG
  - Internet NEG

## Backends service
### named ports
- Required for 
  - Internal and global HTTP(S) Load Balancing
  - SSL Proxy Load Balancing
  - TCP Proxy Load Balancing
- Not required for
  - Internal TCP load balancer
  - Network loadbalancer

### References
- https://cloud.google.com/load-balancing/docs/backend-service
- https://cloud.google.com/load-balancing/docs/ssl-certificates/encryption-to-the-backends
