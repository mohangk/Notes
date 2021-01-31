---
draft: false
title: Using Packer with Terraform on GCP
categories:
  - Terraform
tags:
  - iac
  - gcp
---

## Example config

```json
{
  "builders": [
    {
      "type": "googlecompute",
      "project_id": "my project",
      "source_image": "debian-9-stretch-v20200805",
      "ssh_username": "packer",
      "zone": "us-central1-a",
      "instance_type": "e2-standard-2",
      "instance_name": "etcd-{{timestamp}}"
    }
  ],
  "provisioners": [
    {
        "type": "file",
        "source": "../tf-packer.pub",
        "destination": "/tmp/tf-packer.pub"
      },
    {
        "type": "shell",
        "script": "../scripts/setup.sh"
    }
]
}
```

## API authentication

Several ways to get 
- A service account json referred to  in the `` keyword in the packer json file
- On a machine with the Google Cloud SDK installed enabled via the `gcloud auth application-default login`
- On a VM instance with a service account that has the `computeAdmin` and `serivice account` IAM bindings enabled

## SSH access
- Packer requires SSH to be able to run commands on the instance that will be made into the image


## Reference
- https://www.packer.io/docs/builders/googlecompute
- https://learn.hashicorp.com/tutorials/packer/getting-started-build-image?in=packer/getting-started 