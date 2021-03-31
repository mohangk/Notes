---
draft: false
title: Quick and dirty etcd setup on debian
categories:
  - Databases
tags:
  - etcd
  - high-availability
---
## Key configuration values
###  [member] related configuration

  1. ETCD_NAME - Member name. Defaults to hostname
  2. ETCD_DATA_DIR- Defaults to /var/lib/
  3. ETCD_LISTEN_CLIENT_URLS - what URLS to listen on for clients (must be IP or localhost, cannot be domain name) E.g. http://0.0.0.0:2379
  5. ETCD_ENABLE_V2

### [cluster] related configuration

  1. ETCD_LISTEN_PEER_URLS - what URLS  to listen on for peers (must be IP or localhost, cannot be domain name) E.g. http://0.0.0.0:2380
  2. ETCD_ADVERTISE_CLIENT_URLS - list of this members client URLS to advertise to the cluster. Routable by other E.g. http://10.0.0.1:2379
  3. ETCD_INITIAL_ADVERTISE_PEER_URLS - list of this members peer to advertise to the rest of the cluster. Must be routable by other members. Can be domain. E.g. http://10.0.0.1:2380
  4. ETCD_INITIAL_CLUSTER - List of initial cluster configuration for bootstrapping. Uses the peer URLS. E.g etcd-us-central1-a=http://10.0.0.1:2380, etcd-us-central1-b=http://10.0.0.2:2380, etcd-us-central1-c=http://10.0.0.3:2380
  5. ETCD_INITIAL_CLUSTER_TOKEN 
  6. ETCD_INITIAL_CLUSTER_STATE - 'new' or 'existing'

## Installation

1. Setup 3 instances of etcd each in different zones. E.g.
  - etcd-us-central1-a IP: 10.10.0.6
  - etcd-us-central1-b IP: 10.10.0.7
  - etcd-us-central1-c IP: 10.10.0.8

3. By default etcd will use the environment variables as defined in `/etc/default/etcd`. The systemd unit file in `/usr/lib/systemd/system/etcd.service` and defines 2 defaults that we will use
  - `ETCD_NAME` - hostname
  - `ETCD_DATA_DIR` - /var/lib/etcd/default

4. In the following step we will 
 - Copy over the config to the the instances replacing SELF_IP, HOST_ZONE_A, HOST_ZONE_B, HOST_ZONE_C with the correct IP addresses 
 - When running the sed command make sure that the SELF_IP value is changed to reflect the actual IP for every instance
 - Install and restart etcd-server so it picks up the new configuration

```
ETCD_NAME="SELF_HOST"
ETCD_DATA_DIR=/var/lib/etcd/etcd-cluster1
ETCD_ENABLE_V2=True
ETCD_LISTEN_CLIENT_URLS="http://0.0.0.0:2379"
ETCD_LISTEN_PEER_URLS="http://0.0.0.0:2380"
ETCD_ADVERTISE_CLIENT_URLS="http://SELF_IP:2379"
ETCD_INITIAL_ADVERTISE_PEER_URLS="http://SELF_IP:2380"
ETCD_INITIAL_CLUSTER="etcd-us-central1-a=http://HOST_ZONE_A:2380,etcd-us-central1-b=http://HOST_ZONE_B:2380,etcd-us-central1-c=http://HOST_ZONE_C:2380"                                                           
ETCD_INITIAL_CLUSTER_STATE="new"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster-1"
ETCD_LOG_LEVEL=info
ETCD_DEBUG=False
```

```bash
scp etcd-config.txt 10.10.0.6:~
ssh 10.10.0.6 "sudo apt-get -yq purge etcd-server;"
ssh 10.10.0.6 "sudo apt-get -yq update; sudo apt-get -yq install etcd-server;"
ssh 10.10.0.6 "sudo sed -i 's/%p/etcd/g' /etc/systemd/system/etcd2.service"
ssh 10.10.0.6 "sed 's/SELF_HOST/etcd-us-central1-a/g; s/SELF_IP/10.10.0.6/g; s/HOST_ZONE_A/10.10.0.6/g; s/HOST_ZONE_B/10.10.0.7/g; s/HOST_ZONE_C/10.10.0.8/g' etcd-config.txt |  sudo tee /etc/default/etcd;" 
ssh 10.10.0.6 "sudo systemctl restart etcd"

scp etcd-config.txt 10.10.0.7:~
ssh 10.10.0.7 "sudo apt-get -yq purge etcd-server;"
ssh 10.10.0.7 "sudo apt-get -yq update; sudo apt-get -yq install etcd-server;"
ssh 10.10.0.7 "sudo sed -i 's/%p/etcd/g' /etc/systemd/system/etcd2.service"
ssh 10.10.0.7 "sed 's/SELF_HOST/etcd-us-central1-b/g; s/SELF_IP/10.10.0.7/g; s/HOST_ZONE_A/10.10.0.6/g; s/HOST_ZONE_B/10.10.0.7/g; s/HOST_ZONE_C/10.10.0.8/g' etcd-config.txt | sudo tee /etc/default/etcd;"
ssh 10.10.0.7 "sudo systemctl restart etcd"

scp etcd-config.txt 10.10.0.8:~
ssh 10.10.0.8 "sudo apt-get -yq purge etcd-server;"
ssh 10.10.0.8 "sudo apt-get -yq update; sudo apt-get -yq install etcd-server;"
ssh 10.10.0.6 "sudo sed -i 's/%p/etcd/g' /etc/systemd/system/etcd2.service"
ssh 10.10.0.8 "sed 's/SELF_HOST/etcd-us-central1-c/g; s/SELF_IP/10.10.0.8/g; s/HOST_ZONE_A/10.10.0.6/g; s/HOST_ZONE_B/10.10.0.7/g; s/HOST_ZONE_C/10.10.0.8/g' etcd-config.txt | sudo tee /etc/default/etcd;" 
ssh 10.10.0.8 "sudo systemctl restart etcd"
```

```
 parallel-ssh -h hosts "sudo apt-get update; sudo apt-get install etcd-server;"
 parallel-ssh  -h hosts "sudo sed -i 's/%p/etcd/g' /etc/systemd/system/etcd2.service"
 ssh 10.10.0.6 "sed 's/SELF_HOST/etcd-us-central1-a/g; s/SELF_IP/10.10.0.6/g; s/HOST_ZONE_A/10.10.0.6/g; s/HOST_ZONE_B/10.10.0.7/g; s/HOST_ZONE_C/10.10.0.8/g' etcd-config.txt |  sudo tee /etc/default/etcd;" 
 ssh 10.10.0.7 "sed 's/SELF_HOST/etcd-us-central1-b/g; s/SELF_IP/10.10.0.7/g; s/HOST_ZONE_A/10.10.0.6/g; s/HOST_ZONE_B/10.10.0.7/g; s/HOST_ZONE_C/10.10.0.8/g' etcd-config.txt | sudo tee /etc/default/etcd;"
 ssh 10.10.0.8 "sed 's/SELF_HOST/etcd-us-central1-c/g; s/SELF_IP/10.10.0.8/g; s/HOST_ZONE_A/10.10.0.6/g; s/HOST_ZONE_B/10.10.0.7/g; s/HOST_ZONE_C/10.10.0.8/g' etcd-config.txt | sudo tee /etc/default/etcd;"
 parallel-ssh  -h hosts "sudo systemctl restart etcd"
 etcdctl --endpoints http://10.10.0.6:2379 member list
```

## Running a cluster in the foreground 

```
 etcd --name etcda --initial-advertise-peer-urls http://10.10.0.6:2380 \
  --listen-peer-urls http://10.10.0.6:2380 \
  --listen-client-urls http://10.10.0.6:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://10.10.0.6:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcda=http://10.10.0.6:2380,etcdb=http://10.10.0.7:2380,etcdc=http://10.10.0.8:2380 \
  --initial-cluster-state new

 etcd --name etcdb --initial-advertise-peer-urls http://10.10.0.7:2380 \
  --listen-peer-urls http://10.10.0.7:2380 \
  --listen-client-urls http://10.10.0.7:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://10.10.0.7:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcda=http://10.10.0.6:2380,etcdb=http://10.10.0.7:2380,etcdc=http://10.10.0.8:2380 \
  --initial-cluster-state new

 etcd --name etcdc --debug --initial-advertise-peer-urls http://10.10.0.8:2380 \
  --listen-peer-urls http://10.10.0.8:2380 \
  --listen-client-urls http://10.10.0.8:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://10.10.0.8:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcda=http://10.10.0.6:2380,etcdb=http://10.10.0.7:2380,etcdc=http://10.10.0.8:2380 \
  --initial-cluster-state new 
```


### References:

- https://thenewstack.io/tutorial-set-up-a-secure-and-highly-available-etcd-cluster/
- https://etcd.io/docs/v3.4.0/dev-guide/interacting_v3/