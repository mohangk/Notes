---
draft: true
title: Gke
categories:
  - K8s
---
# GKE

## Setting up a private GKE cluster

### Networking

3 IP ranges - for nodes, pods and services.

#### gcloud

##### setup private GKE cluster

- <ensure you configure nat if you want to access images from other locations - https://medium.com/google-cloud/using-cloud-nat-with-gke-cluster-c82364546d9e>

```
gcloud container clusters create g-zero-gcp \
    --zone us-west1 \
    --enable-master-authorized-networks \
    --master-authorized-networks 118.136.174.61/32 \
    --enable-ip-alias \
    --enable-private-nodes \
    --master-ipv4-cidr 172.20.0.0/28 \
    --no-enable-basic-auth \
    --tags=g-zero-gke \
    --preemptible \
    --max-nodes 1 \
    --num-nodes 1 \
    --machine-type f1-micro

  gcloud container clusters create notes-gcp \
    --zone us-west1 \
    --enable-master-authorized-networks \
    --master-authorized-networks 182.253.250.214/32\
    --enable-ip-alias \
    --enable-private-nodes \
    --master-ipv4-cidr 172.20.0.0/28 \
    --no-enable-basic-auth \
    --tags=notes-gke \
    --max-nodes 3 \
    --num-nodes 3 \
    --machine-type g1-small

gcloud container clusters update nginx --enable-master-authorized-networks --master-authorized-networks 139.0.140.27/32
```

##### setup kubectl to access GKE cluster

https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl

```
gcloud container clusters get-credentials <your-cluster-name> --zone=<your-cluster-zone>
```

##### build local docker image

```
docker build -f Dockerfile gcr.io/mohangk-226507/ut-scrape:1.0 .
```

##### push to gcloud container registry

 gcloud auth configure-docker

docker tag python-test:0.3 gcr.io/mohangk-226507/python-test:0.3

docker push gcr.io/mohangk-226507/python-test:0.3

##### expose the service over NLB

kubectl expose deployment python-test-deployment --name python-test --type=LoadBalancer --port 80 --target-port 5000

##### expose the service over GCLB

##### delete cluster

gcloud container clusters delete <name of cluster>
