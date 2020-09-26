- tis
- expose a different port than what is listening on (targetPort)
- multi region compute clusters
- compute has public ip- why?
- health checks
- custom metrics?
- how is fluent running on the instance
- getting access to the logs on the pods
- how do i know which pod is running on which host?

kubernetes concepts

- pod
- deployment
- service
  - targetPort, port, nodePort
- 

Commands

gcloud container builds submit --tag gcr.io/bbm-development/gs-sprint-boot-docker 

gcloud container clusters create hello-docker \
                --zone us-central1-a \
                --additional-zones us-central1-b,us-central1-c
                --network=NETWORK
                --tags=tag1,tag2

kubectl run hello-docker --image=gcr.io/bbm-development/gs-sprint-boot-docker --port 8080
kubectl get pods

kubectl expose deployment hello-docker --type=LoadBalancer --port 8080 (targetPort??)
 kubectl get service

kubectl scale deployment hello-docker --replicas=2
kubectl logs --tail=30 hello-docker-1550570265-ct9bq
EDITOR=vim kubectl edit pods
kubectl set image deployment/hello-world hello-world=gcr.io/${PROJECT_ID}/hello-node:v2

———

Deleting

kubectl delete service hello-world
gcloud compute forwarding-rules list
gcloud container clusters delete gs-spring-boot-cluster

——

GKE needs scopes - GCS. jenkins will need access to GCR stores content in GCS
gcloud container clusters create jenkins-cd   --network jenkins --machine-type n1-standard-2 --num-nodes 2   --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw,cloud-platform"

—

## Cluster

## Node

- kubelet runs on the node
- kubelet checks health of the pods on the node. Calls a handler that can then execute some command on container, check port or check HTTP endpoint
- additionally can do a livenessProbe (kills the container), readinessProbe (remove from Services)

## Pods

- represents a running process on the cluster
- encapsulates - app container(s), storage, networks IP, governs how the container(s) should run
- unit of deployment - a single instance of the application in kubernetes
- operation modes: one container per pod or multiple pods per container
- runs a single instance of a given application, run multiple pods to scale horizontally
- multi containers are co-located on the same VM
- runs on a node in the cluster. same node until the process is terminated, pod object deleted or evicted or node fails
- started by controller
- you don’t restart pods, you restart containers
- healing etc is handled by controller

### networking

- each pod is assigned a unique IP, share network namespace including the IP address and network pods
- containers inside a pod communicate over localhost
- when comm externally, need to coordinate sharing of network resources

### storage

- set of shared storage volumes
- all containers in the pod can access the shared volumes, can share data, allows for data persistence

## pods and controllers

- manages and creates multiple pods, replication, rollout, self-healing

### podtemplate

- are used by controllers to create pods that its responsible for

### controllers

- Deployment

- StatefulSet

- DaemonSet

- Controllers provide self-healing with a cluster scope, as well as replication and rollout management

#### statefulsets

- Ensure  stable: network identifiers, persistent storage, graceful deployment and scaling, automated rolling updates

- 

#### daemonset

- ensure the pod is run on all nodes in the cluster
