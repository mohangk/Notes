---
draft: false
title: Gcloud
categories:
  - Gcp
---

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

### Setting up Application default credentials

gcloud auth application-default login

https://cloud.google.com/sdk/gcloud/reference/auth/application-default



https://cloud.google.com/iam/docs/creating-managing-service-accounts