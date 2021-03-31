---
draft: true
title: Kubectl
categories:
  - K8s
---
### kubectl

#### configuration

kubectl config

##### GKE permissions

gcloud container clusters get-credentials nginx \
    --zone us-west1-a \
    --project mohangk-226507

#### run

kubectl run <deployment-name> --image=<image> --port=

#### create deployment

kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node

#### get deployments

kubectl get deployments

#### get pod

kubectl get pods

#### expose deployment

Expose a pod, deployment or service

kubectl expose deployment hello-node --type=LoadBalancer --port=8080

##### types

- ClusterIP - internal only service
- NodePort - externally available
  - A NodePort service is the most primitive way to get external traffic directly to your service. NodePort, as the name implies, opens a specific port on all the Nodes (the VMs), and any traffic that is sent to this port is forwarded to the service.
- LoadBalancer
  - A LoadBalancer service is the standard way to expose a service to the internet. On GKE, this will spin up a [Network Load Balancer](https://cloud.google.com/compute/docs/load-balancing/network/) that will give you a single IP address that will forward all traffic to your service.

#### port-forward

make a port on the cluster available to the local machine that kubectl is running

kubectl get services

#### attach

attaches to a process that is already running inside an existing container to see its output

#### exec

executes a command within a container

kubectl exec -it <pod>  <command>

#### label

kubectl label type KEY=VAL KEY1=VAL1

#### scale

kubectl scale --replicas=1 deployments/python-test-deployment

#### setting up a service in minikube

- kubectl apply -f ./deployment.yaml 
- kubectl expose deployment python-test-deployment --type=LoadBalancer --port=5000
- minikube service python-test-deployment

## cheatsheet

kubectl exec -it <podname>

kubectl log --follow <podname>

kubectl cluster-info
