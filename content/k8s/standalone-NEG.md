---
draft: true
title: Standalone NEG
categories:
  - K8s
---
#### TCP proxy connection to container using NEG

Creating a standalone NEG

1. Create a deployment that is exposed over a specific port, say 50000
2. Create a service 
   1. Uses a NEG by using the annotation
      1. cloud.google.com/neg: '{"exposed_ports":{"80":{}}}'
3. Check the NEG

```gcloud beta compute network-endpoint-groups list```

4. Add the firewall rule to allow health check from GCP health check on any instance on the network

```
gcloud compute firewall-rules create my-fwr --network default \
    --source-ranges 130.211.0.0/22,35.191.0.0/16 --allow tcp
```

6. Create a healthcheck

   gcloud beta compute health-checks create tcp my-hc --use-serving-port --global

6. Create a BE service

```
gcloud compute backend-services create my-bes --global \
    --protocol tcp --health-checks my-hc
```

7. Add NEG to BE service 

   ```
   gcloud beta compute backend-services add-backend my-bes --global \
      --network-endpoint-group k8s1-b3e848f4-default-my-service-80-c15c429d \
      --network-endpoint-group-zone us-west1-a \
      --balancing-mode CONNECTION --max-connections-per-endpoint 5
   ```

8. Create a tcp proxy

gcloud compute target-tcp-proxies create my-tp --backend-service my-bes

9. Create a fwd rule for the tcp proxy

   ```
   gcloud compute forwarding-rules create my-for --global \
       --ip-protocol tcp --ports 25 --target-tcp-proxy my-tp
       
   ```



### L7 LB connecting to container using NEG



1. Create a HC

gcloud beta compute health-checks create http my-hc --use-serving-port --global

### L7 LB connecting serving to different deployments using NEG





