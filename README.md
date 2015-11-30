# Provision Docker-Swarm with Ansible on EC2

This playbook allows you to providion a docker-swarm on Amazon EC2 and provides a simple teardown playbook to remove the cluster. The deploy.yml playbook creates a swarm-admin instance, as the control point for the swarm, and a consul node for key/value needed for docker networking.

### Requirements
   * Ansible 1.9+
   * python >= 2.6
   * boto

### Environment Variables
EVs for provisioing on EC2 are located at `vars/external_vars.yml`. Docker-Machine, used to create the swarm, needs a couple extra EVs for provisioing a swarm. Add your specifcs to this file.

### What it does
Running the playbook: `$ ansible-playbook deploy.yml` does the following:

1. Creates  an instance called "Admin-Swarm" with the security group, "swarm-admin".
1. Installs Docker-Engine, Docker-Machine and Docker-Compose on the Admin-Swarm node
1. Creates a swarm-master node and the number of swarm-nodes specified in the EVs
1. Copies the Public key to swarm-master and nodes

### Teardown
First, export your creds on the command line:

``export AWS_ACCESS_KEY_ID='AK123'``

``export AWS_SECRET_ACCESS_KEY='abc123'``
    
Run the playbook: `$ ansible-playbook -i ec2.py teardown.yml` . It does the following:

1. Runs ec2 dynamic inventory script and finds node with tag_role_admin
2. Terminates the consul, swarm-master and swarm nodes
3. Terminates Admin-Swarm instance
