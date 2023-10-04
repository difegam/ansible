# Facts

When we run a playbook, Ansible will gather facts about the hosts in your inventory. This information can be used in your playbooks to make them more flexible and reusable.

Some of the facts that Ansible gathers are:

- `ansible_architecture`
- `ansible_distribution`
- `ansible_distribution_version`
- `ansible_fqdn`
- `ansible_hostname`
- `ansible_interfaces`
- `ansible_kernel`
- `ansible_os_family`
- `ansible_processor`
- `ansible_processor_cores`
- `ansible_processor_count`
- `ansible_processor_threads_per_core`
- `ansible_python_version`
- `ansible_system`
- `ansible_network_interfaces`

See all facts here: [Facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html#ansible-facts)

In the following example we define a task that prints a hello world message. Ansible by default gathers facts about the hosts in your inventory. We can use the `ansible_facts` variable to print all facts.

```yaml
- name: Print hello world
  hosts: all
  tasks:
    - debug:
        msg: ansible_facts # prints all facts
```

Disable gather facts with `gather_facts: no`

```yaml
- name: Print hello world
  hosts: all
  gather_facts: no
  tasks:
    - debug:
        msg: ansible_facts
```

Examples:
`/home/bob/playbooks/inventory`

```ini
localhost ansible_connection=local nameserver_ip=8.8.8.8 snmp_port=160-161
node01 ansible_host=node01 ansible_ssh_pass=caleston123
node02 ansible_host=node02 ansible_ssh_pass=caleston123
[web_nodes]
node01
node02

[all:vars]
app_list=['vim', 'sqlite', 'jq']
user_details={'username': 'admin', 'password': 'secret_pass', 'email': 'admin@example.com'}
```

`/home/bob/playbooks/app_install.yaml`

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Install applications
      yum:
        name: "{{ item }}"
        state: present
      with_items: "{{ app_list }}"
```

`/home/bob/playbooks/user_setup.yaml`

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Set up user
      user:
        name: "admin"
        password: "secret_pass"
        comment: "admin@example.com"
        state: present
```

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Set up user
      user: "{{ user_details }}"
```
