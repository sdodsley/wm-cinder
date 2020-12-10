Installation
============

Copy `purefa_token.py` to the  `modules` directory of the Pure Storage Ansible Collections for FlashArray.

```bash
cp purefa_token.py  ~/.ansible/collections/purestorage/flasharray/plugins/modules/
``` 

Copy playbook `update_cinder.yaml` and template file `cinder.conf.j2` to your working directory.

Operation
=========

Running the playbook will create a new user on the FlashArray called `cinder` with an associated API token.
Once created all the necessary additions will be made to the Cinder configuration file for this array to
become a configured backend.

```bash
ansible-playbook update_cinder.yaml -e "array_ip=<mgmt_vip> protocol=<type> cinder_file=<filename>"
```

where the above pameters are defined by:

* **mgmt_vip**
  The IP address of the FlashArray Management VIP

* **type**
  The dataplane protocol used by the FlashArray for Cinder. Options are ISCSI or FC (default: ISCSI)

* **cinder_file**
  The name and location of the Cinder configuration file (default: `/etc/cinder/cinder.conf`)

After running the playbook, it necessary to restart the Ciner Volume service.

Assumptions
===========

* It is assumed that the default break-glass FlashArray username/password logon has not been changed.
  If this has been changed then amend the playbook YAML file with the correct username/password details
  in the `purefa_token` task of the playbook.
* It is assumed that the new `cinder` user password will be `pureuser`. This can be changed in the playbook if required.

Optional Requirements
=====================

If the OpenStack cluster is running an **Active/Active** Cinder configuration, this playbook must be run
against **BOTH** Cinder configuration files, and both Cinder Volume services must be restarted.
