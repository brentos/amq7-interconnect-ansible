AMQ7 Interconnect PoC
=====================
PoC that creates brokers and interconnect routers. The brokers are in a master-slave configuration using replication (no shared store)

# Setup
## Install Ansible
Follow the instructions for installing Ansible located [here](http://docs.ansible.com/ansible/intro_installation.html)

For RHEL / Fedora, ensure the EPEL repository is configured, and then run:

    sudo yum install ansible

## Create AWS instances
The current Ansible playbook uses 5 AWS instances. Create 5 RHEL 7.2 instances (AMI ID: ami-775e4f16). Give each of these an elastic IP to prevent the IP addresses from changing.

## Clone this repository

    git clone <repo>

## Update the hosts file
Using the public DNS of the 5 servers provisioned above, update the hosts file. There will be 3 routers and 2 brokers in the hosts file. Set the ansible_host variable for each of the servers.

Additionally, set the ansible_ssh_private_key_file to the AWS pem file. If each host has a different pem file, this can be set on a per-host basis as follows:

    routerA ansible_host=<host> ansible_ssh_private_key_file=<key>

## Run the playbook

The following command will set up both the brokers and the routers (the -T flag adds a connection timeout):

    ansible-playbook site.yml -i hosts -T 30

If it is desired to run just one or the other, run one of the following commands:

    ansible-playbook router.yml -i hosts -T 30
    ansible-playbook broker.yml -i hosts -T 30

## Verify broker web console
Navigate to the broker's Hawtio console (user:admin | password: admin)

    http://<broker_master>:8161/hawtio

and verify that the broker has started successfully

## Send / receive messages through the broker

There are two python scripts in this directory, `send.py` and `receive.py`
