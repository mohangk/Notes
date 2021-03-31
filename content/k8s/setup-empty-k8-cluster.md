---
draft: true
title: Setup Empty K8 Cluster
categories:
  - K8s
---
## vBasic K8 cluster with NAT

## networking

gcloud compute networks list

gcloud compute networks subnets list --network default

```
gcloud compute networks subnets describe <subnetname>
```

## spin up an instance

```
gcloud compute instances create example-instance \
    --machine-type g1-small \
    --no-address \
    --zone us-west1-b \
    --tags no-ip
```

## setup firewall rules

```
gcloud compute firewall-rules create custom-network1-allow-ssh \
    --allow tcp:22 \
    --network custom-network1
gcloud compute firewall-rules create custom-network1-allow-internal \
    --allow tcp:1-65535,udp:1-65535,icmp \
    --source-ranges 192.168.1.0/24 \
    --network custom-network1
```

### setup a nat

https://cloud.google.com/vpc/docs/special-configurations#multiple-natgateways

```
gcloud compute addresses create nat-1 --region us-west1

nat_1_ip=$(gcloud compute addresses describe nat-1 \
    --region us-west1 --format='value(address)')
```

```
gsutil cp gs://nat-gw-template/startup.sh .
```

```
gcloud compute instance-templates create nat-1 \
    --machine-type g1-small --can-ip-forward --tags natgw \
    --metadata-from-file=startup-script=startup.sh --address $nat_1_ip
```

```
gcloud compute health-checks create http nat-health-check --check-interval 30 \
    --healthy-threshold 1 --unhealthy-threshold 5 --request-path /health-check
```

```
gcloud compute firewall-rules create "natfirewall" \
    --allow tcp:80 --target-tags natgw \
    --source-ranges "130.211.0.0/22","35.191.0.0/16"
```

```
gcloud compute instance-groups managed create nat-1 --size=1 --template=nat-1 --zone=us-west1-b
```

```
gcloud compute instance-groups managed update nat-1 \
    --health-check nat-health-check --initial-delay 120 --zone us-west1-b
```

```
nat_1_instance=$(gcloud compute instances list |awk '$1 ~ /^nat-1/ { print $1 }')
```

```
gcloud compute routes create natroute1 --destination-range 0.0.0.0/0 \
    --tags no-ip --priority 800 --next-hop-instance-zone us-west1-b \
    --next-hop-instance $nat_1_instance
```

### setup private gke cluster

```
  gcloud container clusters create notes-gcp \
    --zone us-west1-a \
    --enable-master-authorized-networks \
    --master-authorized-networks 182.253.245.118/32,118.136.37.192/32 \
    --enable-ip-alias \
    --enable-private-nodes \
    --master-ipv4-cidr 172.20.0.0/28 \
    --no-enable-basic-auth \
    --tags=app \
    --max-nodes 1 \
    --num-nodes 1 \
    --machine-type g1-small
```

Slightly more elaborate config - disables pod IP masquerading, use the newer GKE version that supports ILB HTTPs routing. The network related details are available in the gke-internal-http-loadbalancing doc

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

```

```

```
gcloud container clusters get-credentials notes-gcp \
    --zone us-west1 \
    --project mohangk-226507
```

Assign the VMs for the cluster to use the NAT instance to route outside the network

```gcloud compute instances add-tags gke-nginx-default-pool-ebb087e4-sgzr --tags=no-ip --zone=us-west1-a```

Update the IPs that can access the cluster

```
gcloud container clusters update notes-gcp --zone us-west1-a --enable-master-authorized-networks --project mohangk-226507 --master-authorized-networks $(curl -s ifconfig.co)/32

e.g.
gcloud container clusters update istio-gke --enable-master-authorized-networks  --master-authorized-networks=182.253.250.204/32,118.136.174.122/32
```

Get the credentials for the new cluster

```
gcloud container clusters get-credentials nginx \
    --zone us-west1 \
    --project mohangk-226507
```
