# Default inventory file

Location

```bash
/etc/ansible/hosts
```

## example

list of hosts

```bash
server1.example.com
server2.example.com
```

Groups

```bash
[webservers]
server1.example.com
server2.example.com
[db]
server3.example.com
server4.example.com
```

# Inventory Parameters

- ansible_host
- ansible_port
- ansible_user
- ansible_ssh_pass
- ansible_ssh_private_key_file

**ansible_host**: This parameter specifies the hostname or IP address of the target host where Ansible will execute tasks. It defines the remote system's network location, allowing Ansible to connect to the correct destination.

```bash
web  ansible_host=server1.example.com  ansible_connection=ssh ansible_user=root # default user is root
db   ansible_host=server2.example.com  ansible_connection=winrm ansible_user=admin
mail ansible_host=server3.example.com  ansible_connection=ssh ansible_ssh_pass=secretPassword
localhost ansible_connection=localhost
```

> `web`, `db`, `mail` are aliases

**ansible_port**: This parameter defines the SSH port number that Ansible should use to connect to the target host. By default, SSH uses port 22, but you can customize this value if your target host uses a different SSH port for communication.

**ansible_user**: This parameter specifies the username that Ansible should use when connecting to the target host via SSH. It determines which user account Ansible will use for authentication and command execution on the remote system.

**ansible_ssh_pass**: If you're not using SSH keys for authentication, this parameter can be used to specify the SSH password for connecting to the remote host. It's essential to secure this information since storing passwords in plaintext poses security risks.

**ansible_ssh_private_key_file**: When SSH key-based authentication is preferred, this parameter allows you to specify the path to the private key file that Ansible should use to authenticate with the target host. This key file is used instead of a password and provides a more secure method of authentication.

# Different inventory format types

- INI
- YAML

```ini
# Sample Inventory File

# Web Servers
web1  ansible_host=server1.company.com  ansible_connection=ssh  ansible_user=root  ansible_ssh_pass=Password123!
web2  ansible_host=server2.company.com  ansible_connection=ssh  ansible_user=root  ansible_ssh_pass=Password123!
web3  ansible_host=server3.company.com  ansible_connection=ssh  ansible_user=root  ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!

# Grouping Servers
[web_servers]
web1
web2
web3

[db_servers]
db1

# Grouping all groups
[all_servers:children]
web_servers
db_servers
```

```ini
# Sample Inventory File

# Web Servers
web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass

# DB Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

[db_nodes]
sql_db1, sql_db2

[web_nodes]
web_node1, web_node2, web_node3

[boston_nodes]
sql_db1, web_node1

[dallas_nodes]
sql_db2, web_node2, web_node3

[us_nodes:children]
boston_nodes, dallas_nodes
```

## YAML

```yaml
# Sample Inventory File

all:
  hosts:
    web_node1:
      ansible_host: web01.xyz.com
      ansible_connection: winrm
      ansible_user: administrator
      ansible_password: Win$Pass
    web_node2:
      ansible_host: web02.xyz.com
      ansible_connection: winrm
      ansible_user: administrator
      ansible_password: Win$Pass
    web_node3:
      ansible_host: web03.xyz.com
      ansible_connection: winrm
      ansible_user: administrator
      ansible_password: Win$Pass
    sql_db1:
      ansible_host: sql01.xyz.com
      ansible_connection: ssh
      ansible_user: root
      ansible_ssh_pass: Lin$Pass
    sql_db2:
      ansible_host: sql02.xyz.com
      ansible_connection: ssh
      ansible_user: root
      ansible_ssh_pass: Lin$Pass

db_nodes:
  hosts:
    sql_db1:
    sql_db2:

web_nodes:
  hosts:
    web_node1:
    web_node2:
    web_node3:

boston_nodes:
  hosts:
    sql_db1:
    web_node1:

dallas_nodes:
  hosts:
    sql_db2:
    web_node2:
    web_node3:

us_nodes:
  children:
    boston_nodes:
    dallas_nodes:
```
