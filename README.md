# Deploying_Zentyal
Automated deploy of Zentyal 6.1 server with Ansible.

Using this simple playbook you will be able to install Zentyal 6.1 on Ubuntu Server 18.04.

## Goal
This deploy method is very interesting in the following scenarios at least:

* Cloud providers like AWS, GCP, etc.
* Virtualization technologies like WVware, Proxmox, etc.
* Massive installations.
* Unattented installations.
* To have the infraestructure as a code.
* Build testing environments.

**A real example:**

As the responsible of the IT of your business, you want to create an stable and secure environment in AWS to deploy Zentyal as a Domain Controller and Mail server. To accoplish this task, you decided to use **Terraform** to have all the infraestructure as a Code and **Ansible** to configure the server. 
Using this way, you will have all the infraestructure documented, saving time in the deployments and also, avoiding human mistakes during the deploy process.

## Requirements before run the playbook

There are certain modification that you will need to do in the configuration files before use this playbook.

1. Set the IP address of the server in the file 'hosts'.

2. Modify the content of the six variables which are defined in the file 'deploy_zentyal.yaml'.

3. Adjust the content of the file 'ansible.cfg' as your needs.

## Running the playbook

Once you have configured the variables it's time to run the playbook. The first step is to ensure that the connection with the server is correct:

    ansible -m ping zentyal_server

If the connection is ok, then, you just need to run the below command:

    cd path/to/the/directory
    
    ansible-playbook deploy_zentyal.yaml 

## Known issues

When you install Zentyal from an existing Ubuntu Server, at the moment you install the **Network** module the DNS resolution will be lost due the initial setup that this module does at its installation. 
This issue is very critical in case you deploy Zentyal in a place where you don't have access to the server through its console when the network isn't configured. So, in these cases you will have to configure the network module **before** rebooting the system or you will lose the access to configure the server. 

There is a workaround in the playbook which allows to recover the DNS resolution after the network module have been installed, but keep in mind that this is temporal, and you will need to configure the module before rebooting. Below are the command which is runned in the playbook:
    
    systemctl start systemd-resolved


