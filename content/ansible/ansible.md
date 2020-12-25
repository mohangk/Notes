# Ansible

## Basics

* include\_\* - dynamic
* import\_\* - static
* Dynamic includes are processed during runtime, at the point in which the task is encountered
* Statc includes - pre-processed at the point of Playbook parsing time\_


## Using Ansible with GCP

### Dynamic inventory
- Ansible documentation - https://docs.ansible.com/ansible/latest/collections/google/cloud/gcp_compute_inventory.html

- Dependencies 
 `pip install requests google-auth`

http://matthieure.me/2018/12/31/ansible_inventory_plugin.html
https://devopscube.com/ansible-dymanic-inventry-google-cloud/
