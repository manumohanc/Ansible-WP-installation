# Installating wordpress with Ansible Playbook
Ansible is an open-source automation tool, or platform, used in configuration management, application deployment, intraservice orchestration, and provisioning. Ansible is used for the multi-tier deployments and it models all of IT infrastructure into one deployment instead of handling each one separately. There are no agents and no custom security architecture is required to be used in the Ansible architecture.
#### Features of Ansible
- We need to run the process only on master machine and it will automatically get transferred to client servers.
- Agentless: You don’t need to install any other software or firewall ports on the client systems you want to automate. You also don’t have to set up a separate management structure.
- Secure and consistent – Since the Ansible uses SSH and Python it is very secure and the operations are flawless.

#### Ansible installation on EC2 instance 

```sh
amazon-linux-extras install ansible2 -y
```
- The configuaration file of ansible will be
```sh
/etc/ansible/ansible.cfg
```
#### Inventory File:
The Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. The default location for the inventory file is /etc/ansible/hosts. You can also create project-specific inventory files in alternate locations.

I have created a custom inventory file for my project..
```sh
vim hosts
```
```sh
[group]
<IP>  ansible_user="ec2-user" ansible_port=22 ansible_ssh_private_key_file="ansible.pem"
```
> [amazon] is the group name
I've created only one client server. If we want to add more, we can add under the same group name if all are on same OS
If the server is different OS, we need to create another group.

> Note: Ansible is using SSH protocol to connect to the client servers.

There are two methods to pass commands to ansible client
1. [Adhoc method](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html) : Ansible ad hoc commands are written for a very specific task. It  is a one-line command that lets you perform basic tasks efficiently without writing playbooks.
The syntax of ad-hoc commands to run is as follows and see some of the ad-hoc commands as well.
```sh
$ ansible [pattern] -m [module] -a "[module options]"
```
2. [Playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html) : Playbooks are files consisting of your written code, and they are written in YAML format, which defines the tasks and executes them through the Ansible. Playbooks may include one or more plays. Plays defines a set of activities or tasks to be run on hosts of inventory file.
- We need to strictly follow the syntax suggested by ansible, otherwise it will throw the error. Let's see in the following, some of the suggestions made by ansible.
- Playbooks always start in a YAML file with three dashes.
- Items that start with a single dash shall be treated as list items.
- Whitespaces matters in ansible, so the items are always defined by indentation.
 
To Run the playbook, we need to execute the command shown as follows.

```sh
$ ansible-playbook <Playbook Name>
```

##### _Checking Ssh Connectivity Between Master & Client With Ping Module_
```sh
# ansible -i hosts group  -m  ping 
```
> I've created a custom host/inventory file with name hosts. 
This will check mentioned command on the server defined in hosts file under the group 'group'

> -m : module name
Ansible uses modules to run instruction. Here ping is the module name

```sh
<IP> | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}
```
> How ansible code works:
  Ansible first convert the code into python, then the code is transferred to client using scp and execute the python script on the remote server. 
  Python module must be installed on the client and master server

If we have mutiple groups, we can specify the group names or we can select all the groups using 'all'
```sh
ansible -i hosts group1,group2  -m  ping 
```

This will perform the command on all the groups mentioned in the inventory file. 
```sh
ansible -i hosts all  -m  ping 
```
This will perform the command on all the groups mentioned in the inventory file. 

## Files Used

- hosts - Hosts file which should have the list of instances to connect to
- main.yaml - Main ansbile playbooks with have the set of instructions
- httpd.conf.tmpl - template of httpd.conf to set a custom port
- main.vars - All variables used in this playbook is listed here
- root.cnf.tmpl - Mysql root.cnf template 
- test.html - Test page to troubleshoot connectivity
- test.php - PHPinfo fil
- vhost.tmpl - Template for vitual host entry
- wp-config.php.tmpl - Template for the wp-config


