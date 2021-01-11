---
draft: false
title: Terraform getting started in GCP
categories:
  - Terraform
tags:
  - iac
---

## Setting up credentials
### Provide a services account file 

1. This can be defined via the `credentials` keyword in the  `google` provider block (refer to the Terraform configuration below)


2. Or by setting the GOOGLE_APPLICATION_CREDENTIALS environment var with the path to the file

### Install the gcloud SDK 

Upon installing the SDK run `gcloud auth application-default login` and complete the login flow.


### References:
- https://cloud.google.com/sdk/gcloud/reference/auth/application-default

## Sample Terraform configuration

The following is a minimal Terraform configuration (saved as main.tf) to setup a VPC network within a project.

```tf
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "3.5.0"
    }
  }
}

provider "google" {

  credentials = file("<NAME>.json")

  project = "<PROJECT_ID>"
  region  = "us-central1"
  zone    = "us-central1-c"
}

resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}
```

## `terraform` block

Allows for defining the specific version of both terraform and any external providers used in this configuration

## `provider` block

Allows for configuring the provider

## `resource "type" "name"` block

- Takes 2 strings, the type and name. The type is prefix with the provider name e.g. google_compute_network. The type and name together form the resource ID in the form type.name e.g. `google_compute_network.vpc_network`. Configuration values are defined within the `resource` block.

- Explicit dependency can be defined between resources by using the `depends_on` keyworks like so `depends_on = [google_storage_bucket.example_bucket]`

- A resource that is "tainted" is considered in a broken state and will be destroyed and re-created on the next run of `terraform apply`

- Can contain a `provisioner "type"` block that provides a way to run configuration on the instance

### Provisioners

There are 2 major type of provisioners 
- `local-exec` executes commands running Terraform, not the VM itself
- `remote-exec` executes commands on the VM itself

E.g.
```tf
  provisioner "local-exec" {
    command = "echo ${google_compute_instance.vm_instance.name}:  ${google_compute_instance.vm_instance.network_interface[0].access_config[0].nat_ip} >> ip_address.txt"
  }
```
- Provisioners are only run at the point of resource creation unless its a "destroy" provisioner that is run at the point of resource deletion.

### Reference
- https://www.terraform.io/docs/provisioners/

## `variables "name"` block

- Variables need to be defined before they can be used. 
E.g.
```tf
variable "zone" {
    type = string
    default = "us-central1-a"
}
```
- Idiomatic approach is to collect all the variable definition into a variables.tf file
- Variable values (if a default is not provided) can be:
  - Captured at the point of execution 
  - Passed in via commandline
  - Defined in a terraform.tfvars file like so
  ```tf
  zone = "us-central1-b"
  ```
## `output "name"` block
- A way to present data back to the user, e.g.
```tf
  value = google_compute_address.vm_static_ip.address
```
- Idiomatic approach is to put all the outputs in to a outputs.tf file 
## Cheatsheet

1. `terraform init` - downloads any dependent modules
2. `terraform plan` - shows what will be run
3. `terrafom apply` - actually runs the change
4. `terraform show` - shows what has been setup 
5. `terraform destory` - removes all the resources. Understand what you are doing before doing this.
6. `terrform taint` - marks a resource as "tainted" i.e. broken. The next time terraform apply is run the resource will be destroyed and re-created.
7. `terraform refresh` - modifies the state file to capture the actual values as is refleceted in the infrastructure. Deals with drift.
8. `terraform output` - display the outputs

## Reading the terraform output

### Identifying in-place change vs destory & re-create change

In the TF output 
- `~` signifies in place change
- `+` indicates an addition
- `-` indicates a removal
- `+/-` indicates a destroy and recreation

### References
- https://learn.hashicorp.com/tutorials/terraform/google-cloud-platform-change?in=terraform/gcp-get-started


## google_compute_instance module

- To ensue the instance doesn't have an ephemeral public IP, do not add the `access_config {}` block. (https://github.com/hashicorp/terraform/issues/12747)

- `service_account.scopes` are set based on the values available here https://cloud.google.com/sdk/gcloud/reference/alpha/compute/instances/set-scopes#--scopes

- Can contain a provisioner block 