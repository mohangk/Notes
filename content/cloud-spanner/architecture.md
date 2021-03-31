---
draft: true
title: Cloud Spanner Architecture
categories:
  - cloud-spanner
---


## Single region vs multi regions
- For single region deployments, all replicas are read-write
- For multi region deployments, replicas can consist of read-only and witness nodes as well
- With multi region deployments, reads might be closer to clients but writes incur higher latency due to the need to persist commits in 2 separate regions
- 2 regions will be designated as the read-write regions, with one of the 2 being a 


### Read write replicas
- Maintain full copy of the data
- Serve reads and can vote whether to commit a write
- Participate in leadership election
- Only type used in single-region instances

### Read only replicas
- Don't participate in elections either for leaders or commit
- Scales up read capacity without increasing the quorom size for writes


### Witness replicas
- Dont support reads but do participate in voting to commit writes
- Makes it easier to achieve quoroms without the need storage and compute resources that are required by read-write replicas
- Do not maintain a full copy of the data
- Vote whether to commit writes

## High level architecture

client --> API servers --> Span servers (Node) --> Storage layer

- Clients connect to API servers and open up sessions that are tied to the API server. The API servers route requests to the righ nodes

## Sizing

- Sizing based on the amount of data, size of each row and the throughput required 
- “If each row has about 1 KB of data, each Cloud Spanner node provides about 7000 QPS read throughput or about 1800 QPS write throughput”
- Each node can store 2 TB of data



### Reference:
https://cloud.google.com/spanner/docs/replication


