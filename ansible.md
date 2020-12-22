# Ansible for Absolute Beginners

Ansible is a simple, powerful and agentless automation tools that can provision a wide variety of scripts through the Ansible Playbook.

[Ansible Docs](https://docs.ansible.com/)

## Ansible Concepts

### Ansible Inventory

To work with multiple servers., Ansible can connect remotely through SSH (Linux) or Powershell Remoting (Windows). It is agentless and so does not require additional installations for this process.

The [Inventory File](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html) stores information about target servers such as an alias or inventory parameters. Entries can also be grouped with configuration for group variable inheritance.

Inventory Parameters Examples:
* `ansible_connection` - ssh/winrm/localhost
* `ansible_port` - 22/5986
* `ansible_user` - root/administrator
* `ansible_ssh_pass` - password

```
# Ansible Inventory


# Database Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

# Web Servers
web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass

# Groups
[db_nodes]
sql_db1
sql_db2

[web_nodes]
web_node1
web_node2
web_node3

[boston_nodes]
sql_db1
web_node1

[dallas_nodes]
sql_db2
web_node2
web_node3

[us_nodes:children]
boston_nodes
dallas_nodes
```

### Ansible Playbooks

An [Ansible Playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html) can run a series of commands in a sequence or deploy and provision to hundreds of VMs as well as network and cluster configuration.

A Playbook is a single YAML file. A play defines a set of activities to be run on hosts. A task is an action be performed on the host. A distinct play is denoted by a top-level dash to mark it as an independent dictionary. A task is an ordered list or array separated by their own dashes.

The `host` parameter indicates which host the tasks will be run on. It can specify any host or group so long as it is specified in the inventory file.

Modules are commands, scripts, services, and other features available out of the box for playbooks.

Playbooks are executed with `ansible-playbook [YAML file]`. Other commands can be found by running `ansible-playbook --help`.

Example Playbook file:
```
-
    name: 'Stop the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Stop the web services on web server nodes'
            command: 'service httpd stop'
-
    name: 'Shutdown the database services on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Shutdown the database services on db server nodes'
            command: 'service mysql stop'
-
    name: 'Restart all servers (web and db) at once'
    hosts: all_nodes
    tasks:
        -
            name: 'Restart all servers (web and db) at once'
            command: '/sbin/shutdown -r'
-
    name: 'Start the database services on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Start the database services on db server nodes'
            command: 'service mysql start'
-
    name: 'Start the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Start the web services on web server nodes'
            command: 'service httpd start'
```

### Ansible Modules
