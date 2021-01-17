---
draft: false
title: Meta arguments
categories:
  - Terraform
tags:
  - iac
---

Available to resources and modules

## count

```tf
resource "aws_instance" "server" {
  count = 4 # create four similar EC2 instances

  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"

  tags = {
    Name = "Server ${count.index}"
  }
}
```

- Creates the multiple instances  of the module / resource - based on `count`
- Creates a `count` object. `count.index` the distinct index number (starting with `0`) corresponding to the instance
- The instances can be addressed as <TYPE>.<NAME>[<INDEX>]
- The value of `count` must be able to be determined before apply stage. For e.g. can't be an API call.

## for_each

```tf

// when used with maps
resource "azurerm_resource_group" "rg" {
  for_each = {
    a_group = "eastus"
    another_group = "westus2"
  }
  name     = each.key
  location = each.value
}

// when used with string lists
module "bucket" {
  for_each = toset(["assets", "media"])
  source   = "./publish_bucket"
  name     = "${each.key}_bucket"
}
```

- Iterates the resource or modules based on the value assed to `for_each`
- Creates an `each` object with the attributes `each.key` and `each.value`
- The instances can be addressed as <TYPE>.<NAME>[<KEY>]
