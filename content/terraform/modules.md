---
draft: false
title: Modules
categories:
  - Terraform
tags:
  - iac
---


## Modules
- Any folder is considered to be a module. When you run terraform within that directory its considered to be the root module
- Terraform commands will only directly use the configuration files in the current directory
- When terrraform encounters a module block, it can load and process files from a different directory
- Modules can be located locally, or accessed in a VCS, HTTPs URLs, Terraform Cloud, Terraform Enterprise private module registries
- When a module is called from a root module it is referred to as the child module

## Using modules

### `module "name"` block

```tf
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "2.21.0"

  name = var.vpc_name
  cidr = var.vpc_cidr

  azs             = var.vpc_azs
  private_subnets = var.vpc_private_subnets
  public_subnets  = var.vpc_public_subnets

  enable_nat_gateway = var.vpc_enable_nat_gateway

  tags = var.vpc_tags
}
```

- The `"name"` of the module is a local name that the calling module can use to refer to the instance of the module
- The module block allows for specifying a list of meta-arguments as defined below
- Any other variables defined within the `{}` are input variables


### Argumens
- `source` argument is mandatory. If provided like the example, will search inregistry. Can also be a local file path or a URL
- `version` is recommended. Works with modules in the terraform registry
- `depends_on`
- `count` - Refer to meta-arguments section 
- `for_each` - Reger to meta-arguments section


## References 
- [Google Cloud Terraform Modules](https://github.com/terraform-google-modules)
- https://www.terraform.io/docs/modules/sources.html
- https://learn.hashicorp.com/tutorials/terraform/module-create
- https://learn.hashicorp.com/tutorials/terraform/module



