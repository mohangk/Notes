---
draft: true
title: Storage
categories:
  - K8s
---
### k8 storage

#### persistent volume vs volume

volumes - live for the life of the pod

persistent volumes - long term storage inside the cluster. exist beyond containers pods and nodes

#### persistent volumes
- use a persistent volume claim to get access to a persistent volume
- can be dynamically provisioned

