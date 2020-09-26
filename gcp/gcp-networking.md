gcloud compute instance-templates create asset-server-template-2017-03-27-1140est --image asset-server2 --network bbm --subnet bbm-staging-application --no-address --tags=private,asset-server

gcloud compute instance-groups managed create asset-server-group-2017-03-27-1140est --region asia-east1 --template asset-server-template-2017-03-27-1140est --size 1
(add ports, add healthcheck)

gcloud backends?
gcloud lbs?

## GCP networking basics

- regions vs zones
- latency btwn zones ~ 5ms
- multi regions ~ at least 2 regions, 160 km apart

## GCE

- snapshots not available across projects, but images are
- create a snapshot, create an image, spin up from image

## google cloud networking basics

- projects are global, vpcs are global
- subnets are per region
- default is an auto mode network that creates a network in each region by default
