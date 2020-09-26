## gcloud multiple accounts

### bootsrap configuration

1. gcloud config set project "project-id"

2. gcloud config set compute/zone "us-central1-c"

3. gcloud auth login

### listing available configurations

1. gcloud config configurations list

### set a config as active

1. gcloud configu configurations activate <project-name>

### initialising multiple accounts

1. gcloud config configurations create testconfig
2. gcloud init

## create a service account

1. gcloud iam service-accounts create mohangk-docs-writer
2. gcloud projects add-iam-policy-binding mohangk --member "serviceAccount:mohangk-docs-writer@mohangk-226507.iam.gserviceaccount.com" --role "roles/owner"

#### ssh-ing into a machine and key forwarding

gcloud compute ssh db-client --ssh-flag="-A"

## spinning up an instance

gcloud compute images list

gcloud compute instances create web \
--image-family ubuntu-os-cloud\
--image-project ubuntu-1804-bionic-v20181222\
--machine-type g1-small

gcloud compute instances create example-instance \
            --image-family rhel-7 --image-project rhel-cloud \
            --zone us-central

gcloud compute instances create web --image-family ubuntu-1804-lts --image-project ubuntu-os-cloud --machine-type g1-small

Created [https://www.googleapis.com/compute/v1/projects/mohangk-226507/zones/us-west1-b/instances/web].
NAME  ZONE        MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP     STATUS
web   us-west1-b  g1-small                   10.138.0.2   35.230.126.154  RUNNING

Add ssh key to metadata
gcloud compute ssh web --ssh-key-file=~/.ssh/id_rsa --ssh-flag="-p 2211"

1. install caddy server and ensure it starts on boot up
   https://gist.github.com/Jamesits/2a1e2677ddba31fae62d022ef8aa54dc

systemd file - /etc/systemd/system/caddy.service

2. get a reserved public IP
   gcloud compute addresses create web-public-ip

3. setup dns for mohangk.org 

4. open up firewall for port tcp & udp for 443, disable default 22 and attach to instance 

https://cloud.google.com/vpc/docs/using-firewalls
https://cloud.google.com/vpc/docs/firewalls

gcloud compute firewall-rules delete default-allow-rdp
gcloud compute firewall-rules create ssh-alt --allow tcp:2211  --description “Replace the default 22 port for ssh”  --direction INGRESS
gcloud compute firewall-rules create quic --allow udp:443  --description “QUIC”  --direction INGRESS

gcloud compute instances add-tags web --tags=quic

5. setup the https for mohangk.org

6. test quic access to the server

7. move the ssh port to some non descrip port

8. crate a snapshot and make image from it

9. spin up a udp lb
   b
