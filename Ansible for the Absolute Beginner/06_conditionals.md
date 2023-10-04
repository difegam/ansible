# Conditional Statements

Ansible provides a number of conditional statements that help to control the flow of execution of tasks. These statements are similar to the conditional statements found in most programming languages.

## When

We can use the `when` statement to control the execution of a task. The task will only be executed if the condition is true.

```yaml
- name: Install nginx
  yum:
    name: nginx
    state: present
  when: ansible_os_family == "RedHat"
- name: Install nginx
  apt:
    name: nginx
    state: present
  when: ansible_os_family == "Debian"
```

## Or & And & Not Operators

We can use the `or` and `and` operators to combine multiple conditions. We can also use the `not` operator to negate a condition.

```yaml
# or
- name: Install nginx
  yum:
    name: nginx
    state: present
  when: ansible_os_family == "RedHat" or ansible_os_family == "SUSE"

# and
- name: Install nginx
    apt:
        name: nginx
        state: present
    when: ansible_os_family == "Debian" and ansible_distribution_version == "18.04"
```

# Conditional in loops

We can use the `when` statement in loops to control the execution of a task. The task will only be executed if the condition is true.

```yaml
---
- name: Install Softwares
  hosts: all
  vars:
    packages:
      - name: nginx
        required: true
      - name: mysql
        required: true
      - name: apache
        required: false
  tasks:
    - name: Install {{ item.name }} on Debian
      yum:
        name: "{{ item.name }}"
        state: present
      when: item.required == true
      loop: "{{ packages }}"
```

```yaml
---
- name: Check status of a service and email if its down
  hosts: all
  tasks:
    - command: systemctl status nginx
      register: nginx_status
    - mail:
        to: admin@example.com
        subject: "Nginx is down"
        body: "Nginx is down on {{ inventory_hostname }}"
        when: nginx_status.stdout.find("active (running)") == -1 # returns -1 if not found
        # when: nginx_status.stdout.find('down') != -1)
```
# Examples


## Example 1

```yaml
---
- name: "Am I an Adult or a Child?"
  hosts: localhost
  vars:
    age: 25
  tasks:
    - name: I am a Child
      command: 'echo "I am a Child"'
      when: "age < 18"
    - name: I am an Adult
      command: 'echo "I am an Adult"'
      when: "age >= 18"
```

Run the playbook:

```shell
cd /dir/playbooks
ansible-playbook -i inventory age.yaml
```
## Example 2
```yaml
---
- name: "Add name server entry if not already entered"
  hosts: localhost
  become: yes
  tasks:
    - shell: "cat /etc/resolv.conf"
      register: command_output
    - shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
      when: 'command_output.stdout.find("10.0.250.10") == -1'
```

```shell
cd /dir/playbooks
ansible-playbook -i inventory nameserver.yaml
```
## Example 3