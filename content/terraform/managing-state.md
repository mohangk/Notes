---
draft: false
title: Terraform managing state
categories:
  - Terraform
tags:
  - iac
---

## Key commands

- `terraform state list` - list out all resources in the state 
- `terraform state show <resource name>` - display the resource details stored in the state file
- `terraform taint/untaint <resource name>` - set a resource to be destroyed and recreated
- `terraform refresh` - Compare the terraform state with what is actually running. This is run automatically every time `plan` or `apply` is run

## Working around issues
- Edit the resource using CLI or web console and execute a `terraform refresh`