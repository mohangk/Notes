---
draft: true
title: Instance Metadata
categories:
  - Gcp
tags:
  - google-compute-engine
---
## instance metadata

### Set  metada when creating instance

```bash
gcloud compute instance create test
 --metadata=PARAM_NAME1=value1,PARAM_NAME2=value2
```


### Querying the metadata

```bash
curl "http://metadata.google.internal/computeMetadata/v1/instance/disks/?recursive=true" -H "Metadata-Flavor: Google"  

[{"deviceName":"boot","index":0,"mode":"READ_WRITE","type":"PERSISTENT"},  
{"deviceName":"persistent-disk-1","index":1,"mode":"READ_WRITE","type":"PERSISTENT"},  
{"deviceName":"persistent-disk-2","index":2,"mode":"READ_ONLY","type":"PERSISTENT"}]
```

### References: 
- https://cloud.google.com/compute/docs/storing-retrieving-metadata
