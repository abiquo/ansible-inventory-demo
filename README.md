# Abiquo inventory script demo

Derived from the work from https://github.com/ansible/ansible-examples/tree/master/wordpress-nginx_rhel7

Small demo of an ansible playbook run using the Abiquo inventory script.

- Requires Ansible 1.2 or newer
- Expects CentOS/RHEL 7.x host/s

# Requirements

The Abiquo inventory script requires the `abiquo-api` and `requests_oauthlib` libraries installed. You can get them using pip:

`pip install abiquo-api requests_oauthlib`

# Configuration

Edit the config file at `inventory/abiquo_inventory.ini` and set the appropriate variables. You will need to set the Abiquo API URI as well as appropriate authentication.

# Usage

In Abiquo, create a vApp with 2 VMs using a CentOS 7 template. In this VMs you will need to set:

- Public IP if you are running this playbook from outside the cloud.
- Appropriate firewall rules. Recommended:
	- Server: Allow incoming 22/tcp, 80/http from 0.0.0.0/0
	- DB: Allow 22/tcp from 0.0.0.0/0, 3306 from Server network (if different, otherwise same network).
- Set the appropriate variables
	- In the Server, set variable with key `wprole` and value `srv`.
	- In the DB, set variable with key `wprole` and value `db`.

The inventory script groups VMs by variable and value, hence the setup will result in 2 groups, `var_wprole_srv` and `var_wprole_db` which are used in the inventory.

Once you have deployed the VMs, you can check connectivity by running:

`ansible -i inventory wordpress-* -m ping`

Once you've verified connectivity you can run the playbook by running:

`ansible-playbook -i inventory site.yml`

And watch magic being done :)

# Authors

* Author:: Marc Cirauqui (marc.cirauqui@abiquo.com)
