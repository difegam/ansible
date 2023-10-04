# Variables

Variables are used to store values. A variable is created the moment you first assign a value to it.

use `{{ variable_name }}` to use a variable.

```yaml
name: Add DNS server to resolv.conf
hosts: localhost
vars:
  dns_server: 10.1.140.10
tasks:
  - lineinfile:
      path: /etc/resolv.conf
      line: "nameserver {{ dns_server }}"
```

```yaml
- name: "Add nameserver in resolv.conf file on localhost"
  hosts: localhost
  become: yes
  tasks:
    - name: "Add nameserver in resolv.conf file"
      lineinfile:
        path: /etc/resolv.conf
        line: "nameserver {{ nameserver_ip }}"
```

```yaml
- hosts: localhost
  vars:car
    car_model: BMW M3
    country_name: the USA
    title: Systems Engineer
  tasks:
    - command: 'echo "My car is {{ car_model }}"'
    - command: 'echo "I live in {{ country_name }}"'
    - command: 'echo "I work as a Systems {{ title }}"'
```

## variable precedence:

- 0 Group variables
- 1 Host variables
- 2 playbook variables
- 3 extra variables

The highest precedence is given to variables defined in the play itself. These variables can be overridden by any other variable, including inventory variables.

Group variables are overridden by host variables, which are overridden by playbook variables, which are overridden by `extra vars` and `vars_prompt` variables.

````yaml

`ect/ansible/host`

```ini
web1 ansible_host=172.20.1.100
web2 ansible_host=172.20.1.101
web3 ansible_host=172.20.1.102

[web_servers]
web1
web2
web3

[web_servers:vars]
dns_server=10.5.5.3

````

See full list of precedence here: [Understanding variable precedence](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)

```yaml
- name: Check /etc/host file
  hosts: all
  tasks:
    - shell: cat /etc/hosts
      register: result # register the output of the command in a variable
    - debug:
        var: result # print the output of the command
```

```yaml
- name: Check /etc/host file
  hosts: all
  tasks:
    - shell: cat /etc/hosts
      register: result # register the output of the command in a variable
    - debug:
        var: result.rc # print the output of the command
```

## Register Scope:

The scope of the variable is the host for which the task is run. If the task is run on multiple hosts, the registered variable will be overwritten for each host.

## Magic Variables:

Magic variables are global variables that are automatically set by Ansible and can be used in tasks and templates.

### Magic Variable - Hostvars:

Magic variable hostvars returns all variables for a given host.

```yaml
- name: Print dns server
  hosts: all
  tasks:
    - debug:
        msg: "DNS server is {{ hostvars['web2'].dns_server }}"
```

Get Ip address of the host:

```yaml
- name: Print IP address of remote host
  hosts: all
  tasks:
    - debug:
        msg: "IP address  {{ hostvars['web2'].ansible_host }}"
        msg: "architecture  {{ hostvars['web2'].ansible_fact.architecture }}"
        msg: "devices  {{ hostvars['web2'].ansible_fact.devices }}"
        msg: "mounts  {{ hostvars['web2'].ansible_fact.mounts }}"

        msg: "processor dot notation  {{ hostvars['web2'].ansible_fact.processor }}"
        msg: "processor brackets & single quotes {{ hostvars['web2']['ansible_fact']['processor'] }}"
```

### Magic Variable - Groups:

Magic variable groups return all hosts under a given group.

```yaml
msg: "{{ groups['web_servers'] }}"

# output
$ web1
$ web2
$ web3
```

### Magic Variable - Group_names:

Magic variable group_names returns all groups for a given host.

```yaml
- name: Print groups for a given host
  hosts: web1
  tasks:
    - debug:
        msg: "{{ group_names }}"
```

output:

```yaml
$ web_servers
```

### Magic Variable - Inventory_hostname:

Magic variable inventory_hostname returns the name of the current host.

```yaml
- name: Print inventory hostname
  hosts: web1
  tasks:
    - debug:
        msg: "{{ inventory_hostname }}"
```

output:

```yaml
$ web1
```

see all magic variables here: [Magic Variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html#information-about-ansible-magic-variables)
