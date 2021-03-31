---
draft: true
title: Viewing health check logs
categories:
  - Gcp
---


## Enabling health check logs 

```bash
//enable
gcloud compute health-checks update PROTOCOL HEALTH_CHECK_NAME \
    --enable-logging

//disable
gcloud compute health-checks update PROTOCOL HEALTH_CHECK_NAME \
    --no-enable-logging
```

## Viewing the logs

- 2 Key things to know is that:
  - the health check logs are indexed by the NEG or instance group -- not the actual health check
  - only health check transition state is logged, not the result of every health check
- So search for the NEG or instance group resource NOT the health check
- E.g. Instance group search query in log viewer
```
logName="projects/PROJECT_ID/logs/compute.googleapis.com%2Fhealthchecks"  AND
resource.type="gce_instance_group" AND
resource.labels.instance_group_name="INSTANCE_GROUP_NAME"
```
- For NEGs
```
logName="projects/PROJECT_ID/logs/compute.googleapis.com%2Fhealthchecks"  AND
resource.type="gce_network_endpoint_group" AND
resource.labels.network_endpoint_group_id="ENDPOINT_GROUP_ID"
```


### Reference:
- https://cloud.google.com/load-balancing/docs/health-check-logging