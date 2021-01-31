---
draft: true
title: Gcp Networking
categories:
  - Gcp
---
## GCP networking basics

- regions vs zones
- latency btwn zones ~ 5ms
- multi regions ~ at least 2 regions, 160 km apart

## GCE

- snapshots not available across projects, but images are
- create a snapshot, create an image, spin up from image

## google cloud networking basics

- projects are global, vpcs are global
- subnets are per region
- default is an auto mode network that creates a network in each region by default

##### Internal DNS
https://cloud.google.com/compute/docs/internal-dns

###### Customise the search path ot include all zones

Add the following to /etc/dhcp/dhclient.conf

append domain-search "us-central1-b.c.gcplabtest-286209.internal.", "us-central1-c.c.gcplabtest-286209.internal."; 
Reload the config by doing - sudo dhclient -r ; dhclient 

##### VmDNSsetting 
- Determine what the internal FQDN for an instance will be
https://cloud.google.com/compute/docs/internal-dns
gcloud compute project-info add-metadata --metadata=VmDnsSetting=ZonalOnly|GlobalOnly



References:
- 
- https://stackoverflow.com/questions/25313049/configuring-fqdn-for-gce-instance-on-startup
