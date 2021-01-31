---
draft: false
title: Terraform getting started in GCP
categories:
  - Terraform
tags:
  - iac
  - gcp
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

## Cheatsheet

1. `terraform init` - downloads any dependent modules
2. `terraform plan` - shows what will be run
3. `terrafom apply` - actually runs the change
4. `terraform show` - shows what has been setup 
5. `terraform destory` - removes all the resources. Understand what you are doing before doing this.
6. `terrform taint` - marks a resource as "tainted" i.e. broken. The next time terraform apply is run the resource will be destroyed and re-created.
7. `terraform refresh` - modifies the state file to capture the actual values as is refleceted in the infrastructure. Deals with drift.
8. `terraform output` - display the outputs
9. `terraform import <resource.name> name-of-resource` - allows you to import existing infrastructure into your tfstate. The resource needs to have already been defined in your tf config

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

## google_compute_firewall
```tf
resource "google_compute_firewall" "default" {
  name    = "test-firewall"
  network = google_compute_network.default.name

  allow {
    protocol = "icmp"
  }

  allow {
    protocol = "tcp"
    ports    = ["80", "8080", "1000-2000"]
  }

  source_tags = ["web"]
}
```

## google_compute_region_instance_group_manager

- Manage a regional MIG

```tf
resource "google_compute_health_check" "autohealing" {
  name                = "autohealing-health-check"
  check_interval_sec  = 5
  timeout_sec         = 5
  healthy_threshold   = 2
  unhealthy_threshold = 10 # 50 seconds

  http_health_check {
    request_path = "/healthz"
    port         = "8080"
  }
}

resource "google_compute_region_instance_group_manager" "appserver" {
  name = "appserver-igm"

  base_instance_name         = "app"
  region                     = "us-central1"
  distribution_policy_zones  = ["us-central1-a", "us-central1-f"]

  version {
    instance_template = google_compute_instance_template.appserver.id
  }

  target_pools = [google_compute_target_pool.appserver.id]
  target_size  = 2

  named_port {
    name = "custom"
    port = 8888
  }

  auto_healing_policies {
    health_check      = google_compute_health_check.autohealing.id
    initial_delay_sec = 300
  }
}
```


## Reference
- [Google Cloud provider docs](https://registry.terraform.io/providers/hashicorp/google/latest/docs)


