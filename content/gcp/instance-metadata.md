---
draft: true
title: Instance Metadata
categories:
  - Gcp
tags:
  - google-compute-engine
---
## instance metadata

#### Set the right scopes when creating the

--scopes=https://www.googleapis.com/auth/compute.readonly,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/trace.append,https://www.googleapis.com/auth/devstorage.read_write 

#### Querying the metadata

curl "http://metadata.google.internal/computeMetadata/v1/instance/disks/?recursive=true" -H "Metadata-Flavor: Google"  

[{"deviceName":"boot","index":0,"mode":"READ_WRITE","type":"PERSISTENT"},  
{"deviceName":"persistent-disk-1","index":1,"mode":"READ_WRITE","type":"PERSISTENT"},  
{"deviceName":"persistent-disk-2","index":2,"mode":"READ_ONLY","type":"PERSISTENT"}]

References: 
https://cloud.google.com/compute/docs/storing-retrieving-metadata
