---
draft: true
title: Service Accounts
categories:
  - Gcp
---
### intro

- belongs to application or virtual machine
- service account key pairs are associated with a service account
- key attributes
  - what resources
  - what permissions
  - where will it run?
- iam service account 
  - both a resource and identity
  - as identity, it can have roles applied to it
  - as a resource - grant permission to a user to access the service account
    - Grant the Service Account User (iam.serviceAccountUser) role for the service account to your employees who need permission to start the job. In this scenario, the service account is the resource

#### Creating a service account

```
gcloud iam service-accounts create
gcloud projects add-iam-policy-binding _ --member "serviceAccount:<name>@<project_id>.iam.gserviceaccount.com" --role "roles/owner"
gcloud iam service-accounts keys create .json --iam-account <name>@<project_id>.iam.gserviceaccount.com
```
