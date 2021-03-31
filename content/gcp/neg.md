---
draft: true
title: NEG - Network endpoint groups
categories:
  - Gcp
---
#### Overview

- NEG - group of backends served by a lb

- List of IPs managed by a NEG controller, can be primary or secondary IPs - hence can communicate directly with pods. Container native networking, allows for health checks from the LB to the pods, better for rolling deploys than just using readiness probes

##### Ingress with NEGs

- Ingress controller sets up the NEG - creating VIP, forwarding rules, health checks, firewall rules etc - can setup with either internal or external LB

##### Standalone NEGs

- Manually setup the L7 LB

- Can be setup to be used by Traffic director, external/internal HTTPs LB
