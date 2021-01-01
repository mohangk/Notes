---
draft: false
title: Ansible GCP Dynamic inventory
categories:
  - Ansible
---

## What?

Allows for the auto discovery of VMs from within Ansible so playbooks can directly refer to them

## Basic steps

1. Ensure the dependencies are installed
```bash
pip install requests google-auth
```
2. Enable gcp_compute plugin in ansible.cfg
```ini
[inventory]
enable_plugins = gcp_compute
```

3. Create a file that ends with "gcp.yml" (e.g inv.gcp.yml) with the following contents (remembering to replace with the actual project_id). In this example we use the default service account on the machine. 

```yaml
plugin: gcp_compute
projects:
  - REPLACE_WITH_PROJECT_ID
auth_kind: machineaccount
service_account_email: default
keyed_groups:
  - key: labels
hostnames:
  - name
```

4. Test it as follows 
```bash
ansible-inventory --list -i inv.gcp.yml
```


## References
  - [Ansible documentation on GCP dynamic inventory](https://docs.ansible.com/ansible/latest/collections/google/cloud/gcp_compute_inventory.html)
  - http://matthieure.me/2018/12/31/ansible_inventory_plugin.html
  - https://devopscube.com/ansible-dymanic-inventry-google-cloud/
