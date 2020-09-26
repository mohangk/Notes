```
gcloud beta container clusters create $CLUSTER --zone $ZONE \    --addons Istio \    --istio-config auth=MTLS_PERMISSIVE
```

```
gcloud container clusters create istio-gke  --zone us-west1-a \ 
--node-locations us-west1-a \ 
--enable-master-authorized-networks \ 
--master-authorized-networks 114.124.161.130/32 \
--enable-ip-alias\
--enable-private-nodes\ 
--master-ipv4-cidr 172.21.0.0/28\  
--no-enable-basic-auth\  
--tags=istio-gke\ 
--max-nodes 1\ 
--num-nodes 1\
--machine-type g1-small\ 
--addons Istio\
--istio-config auth=MTLS_PERMISSIVE
```

### Different update approaches

#### Recreate

#### RollingUpdates

##### Config options

- maxSurge
- maxUnavailable

#### Lifecyle

##### preStop

#### Probes

- liveness : when to restart a container 
- readiness : when to start accepting traffic
- startup: when an application is started. liveness and readiness checks is disabled until startup is running
- Options for probes:
- httpGet:
  
          path: /version
          port: 8080
- command:
  
          path: /version
          port: 8080



##### Canary using Istio

- Gateway - functions as an ingress 

- VirtualServices - handles the routing

[https://medium.com/faun/istio-step-by-step-part-04-traffic-routing-path-of-istio-service-mesh-part-a-ingress-routing-28e03cdaa048](https://medium.com/faun/istio-step-by-step-part-04-traffic-routing-path-of-istio-service-mesh-part-a-ingress-routing-28e03cdaa048)
