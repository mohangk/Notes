---
draft: true
title: Gke Internal Http Loadbalancing
categories:
  - Gcp
---
##### Ingress for Internal HTTP(s) Load balancing

- need to setup a proxy only subnet of at least 64 IP - /26

- only on proxy-only subnet per region, per VPC can be active

###### Setting up proxy-only-network network & subnets

```
gcloud compute networks create lb-network --subnet-mode=custom

gcloud compute networks subnets create backend-subnet \
    --network=lb-network \
    --range=10.1.2.0/24 \
    --region=us-west1

gcloud compute networks subnets create proxy-only-subnet \
  --purpose=INTERNAL_HTTPS_LOAD_BALANCER \
  --role=ACTIVE \
  --region=us-west1 \
  --network=lb-network \
  --range=10.129.0.0/23
```

###### Setting up firewall rules

```
gcloud compute firewall-rules create fw-allow-ssh \
    --network=lb-network \
    --action=allow \
    --direction=ingress \
    --target-tags=allow-ssh \
    --rules=tcp:22

gcloud compute firewall-rules create fw-allow-health-check \
    --network=lb-network \
    --action=allow \
    --direction=ingress \
    --source-ranges=130.211.0.0/22,35.191.0.0/16 \
    --target-tags=lb-backends \
    --rules=tcp

gcloud compute firewall-rules create fw-allow-proxies \
  --network=lb-network \
  --action=allow \
  --direction=ingress \
  --source-ranges=10.129.0.0/23 \
  --target-tags=lb-backends \
  --rules=tcp:80,tcp:443,tcp:8080
```

Setup GKE Cluster

```
gcloud container clusters create prod-cluster \
    --release-channel=rapid \
    --disable-default-snat \
    --region=us-central1 \
    --enable-master-authorized-networks \
    --master-authorized-networks=116.86.103.128/32 \
    --enable-ip-alias \
    --enable-private-nodes \
    --master-ipv4-cidr 172.20.0.0/28 \
    --no-enable-basic-auth \
    --max-nodes=3 \
    --num-nodes=1\
    --machine-type=n1-standard-1\
    --network=production-network \
    --subnetwork=backend-subnet\
    --tags=lb-backends
```

Reference:

1. [Ingress for Internal HTTP(S) Load Balancing](https://cloud.google.com/kubernetes-engine/docs/concepts/ingress-ilb)https://cloud.google.com/load-balancing/docs/l7-internal/setting-up-l7-internal#configure-a-network

2. https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balance-ingress
