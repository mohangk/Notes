

### Why NEG and not plain ingress using node ports?



https://chinagdg.org/2018/10/introducing-container-native-load-balancing-on-google-kubernetes-engine/

NEG - container native, doesnt need node port - goes straight to the pods



Load balancers were initially built to support resource allocation for virtual machines, helping make workloads highly available. When containers and container orchestrators started to take off, users adapted these VM-focused load balancers for their use, even though the performance was suboptimal. Container orchestrators like GKE performed load balancing using existing network functions applied to the granularity of VMs. In the absence of a way to define a group of pods as backends, the load balancer used instance groups to group VMs as backends. Ingress support in GKE using those instance groups used the HTTP(S) load balancer to perform load balancing to nodes in the cluster. Those nodes followed IPTable rules to route the requests to the pods serving as backends for the load-balanced application. Load balancing to the nodes was the only option, since the load balancer didn’t recognize pods or containers as backends, resulting in imbalanced load and a suboptimal data path with lots of traffic hops between nodes.

The new NEG abstraction layer that enables container-native load balancing is integrated with the Kubernetes Ingress controller running on GCP. If you have a multi-tiered deployment where you want to expose one or more services to the internet using GKE, you can also create an [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) object, which provisions an HTTP(S) load balancer, allowing you to configure path-based or host-based routing to your backend services.

----



[kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/) configures nodes' `iptables` rules to distribute traffic to Pods. Without container-native load balancing, load balancer traffic travels to the node instance groups and gets routed via `iptables` rules to Pods which might or might not be in the same node. With container-native load balancing, load balancer traffic is distributed directly to the Pods which should receive the traffic, eliminating the extra network hop. Container-native load balancing also helps with improved health checking since it targets Pods directly.



### Digging into NEG 

Goal: Setup HTTP(s) LB using a NEG

https://cloud.google.com/load-balancing/docs/negs/

LB -> FWDing rules - > BE -> NEG(s) -> PODS



