---
draft: false
title: Terraform language quickstart
categories:
  - Terraform
tags:
  - iac
---

## String interpolation
- `"Hello, ${var.name}!"`

## Important concepts
- Input variables are like function arguments.
- Output values are like function return values.
- Local values are like a function's temporary local variables.

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

## `data "data source" "name"` block
- Data sources allow data to be fetched or computed and stored to be used 
- 

### Reference
- https://www.terraform.io/docs/configuration/data-sources.html

## `locals` block 
- Allows for the definition of local variables that can be used in multiple places
- Not limited to literals, can be expressions or other variables
- E.g.

```tf

locals {
  # Ids for multiple sets of EC2 instances, merged together
  instance_ids = concat(aws_instance.blue.*.id, aws_instance.green.*.id)
}

```

## `dynamic "varname"` block
- Expressions can only be used in the top level block constructs when assigning a value to an argument. 
- You can dynamically construct repeatable nested blocks like setting using a special dynamic block type, which is supported inside resource, data, provider, and provisioner blocks
- The parameter passed into the block becomes the name of the variable in the subblock
- E.g.
```tf
resource "aws_elastic_beanstalk_environment" "tfenvtest" {
  name                = "tf-test-name"
  application         = "${aws_elastic_beanstalk_application.tftest.name}"
  solution_stack_name = "64bit Amazon Linux 2018.03 v2.11.4 running Go 1.12.6"

  dynamic "setting" {
    for_each = var.settings
    content {
      namespace = setting.value["namespace"]
      name = setting.value["name"]
      value = setting.value["value"]
    }
  }
}
```

## Collection functions
- `lookup` - returns the single value from a map with the ability to have a default - `lookup(map, key, default)` 