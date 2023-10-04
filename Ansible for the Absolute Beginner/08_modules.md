# Modules

## What is a module?

A module is a program that performs a specific task. Ansible modules are reusable, standalone scripts that can be used by the Ansible API, or by the ansible or ansible-playbook programs. Ansible ships with a number of modules (called the ‘module library’) that can be executed directly on remote hosts or through Playbooks. Ansible modules are written in Python and can be executed directly on remote hosts or through Playbooks.

## Modules categories

Ansible modules are organized into categories. The categories are:

- System
- Commands
- Files
- Database
- Cloud
- Windows
- Utilities
- ...

See [Ansible modules](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html)

### System

System modules are used to manage system resources, such as services, users, and groups. Some examples of system modules are:

- [service](https://docs.ansible.com/ansible/latest/modules/service_module.html): Manage services.
- [user](https://docs.ansible.com/ansible/latest/modules/user_module.html): Manage user accounts.
- [group](https://docs.ansible.com/ansible/latest/modules/group_module.html): Manage groups.
- [cron](https://docs.ansible.com/ansible/latest/modules/cron_module.html): Manage cron.d and crontab entries.
- [hostname](https://docs.ansible.com/ansible/latest/modules/hostname_module.html): Manage hostname.
- [mount](https://docs.ansible.com/ansible/latest/modules/mount_module.html): Manage filesystem mounts.
- [ping](https://docs.ansible.com/ansible/latest/modules/ping_module.html): Try to connect to host, verify a usable python and return pong on success.
- [reboot](https://docs.ansible.com/ansible/latest/modules/reboot_module.html): Reboot a machine.
- [shell](https://docs.ansible.com/ansible/latest/modules/shell_module.html): Execute shell commands on targets.
- [systemd](https://docs.ansible.com/ansible/latest/modules/systemd_module.html): Manage systemd services.
- [wait_for](https://docs.ansible.com/ansible/latest/modules/wait_for_module.html): Waits for a condition before continuing.
- [yum](https://docs.ansible.com/ansible/latest/modules/yum_module.html): Manages packages with the yum package manager.
- [apt](https://docs.ansible.com/ansible/latest/modules/apt_module.html): Manages apt-packages.

**Service module**
Service module is used to manage services on remote hosts. Some examples of service modules are:

Manage the state of a service:

```yaml
- name: Start the service
  hosts: localhost
  tasks:
   - name: Start the service
     service:
       name: postgresql
       state: started
```

```yaml
- name: Start the service
  hosts: localhost
  tasks:
   - name: Start the service
     service: name=postgresql state=started
```
---
> **Why `started` and not `start`?**.  Because the service module checks the state of the service and only starts it if it is not already started. **We are not** instructing ansible **to start** the service, we are instructing ansible **to ensure that the service is started**.


A key principle of Ansible that makes it different from scripting is the concept of **idempotency**. The official Ansible documentation describes idempotency as follows: “An operation is idempotent if the result of performing it once is exactly the same as the result of performing it repeatedly without any intervening actions.” This means that if you run a playbook with the same set of inputs, you should not expect it to make any changes on the system.

- If the service is already started, the task will be marked as `ok` and will not be executed again.

---


### Commands

Command modules are used to execute commands on remote hosts. Some examples of command modules are:

- [command](https://docs.ansible.com/ansible/latest/modules/command_module.html): Execute commands on targets.
- [raw](https://docs.ansible.com/ansible/latest/modules/raw_module.html): Executes a low-down and dirty SSH command.
- [script](https://docs.ansible.com/ansible/latest/modules/script_module.html): Runs a local script on a remote node after transferring it.
- [shell](https://docs.ansible.com/ansible/latest/modules/shell_module.html): Execute shell commands on targets.
- [expect](https://docs.ansible.com/ansible/latest/modules/expect_module.html): Executes a command and responds to prompts.

**[Command Parameters](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#parameters)**
parameter | required | default | choices | comments
--- | --- | --- | --- | ---
argv | no | | | Passes the command as a list rather than a string.
chdir | no | | | Change into this directory before running the command.
creates | no | | | A filename, when it already exists, this step **will not** be run.
executable | no | | | Change the shell used to execute the command.
free_form | yes | | | The command module takes a free form string as a command to run.
removes | no | | | A filename, when it does not exist, this step will not be run.
stdin | no | | | Set the stdin of the command directly to the specified value.
warn | no | yes | yes/no | Whether to enable task warnings.

**Example**

```yaml
- name: Run a command that uses stdin module
  hosts: localhost
  tasks:
    - name: Execute a command date
      command: date
      register: result

    - name: Print the result
      debug:
        msg: "{{ result.stdout }}"

    - name: Display resolv.conf file
      command: cat /etc/resolv.conf # command -> run a command on the remote host

    - name: Display resolv.conf file
      command: cat resolv.conf chdir=/etc # chdir -> change directory before running the command

    - name: Display resolv.conf file
      command: cat resolv.conf chdir=/etc warn=no

    - name: Display resolv.conf contents
      command: mkdir /folder creates=/folder # creates -> if folder exists, this step will not be run
```
**Script module**
Run a local script on a remote node after transferring it.

The steps involved in using the script module are:
1. Copy the script to the remote node
2. Execute the script on the remote node
3. Remove the script from the remote node


```yaml
- name: Run a script on the remote node
  hosts: all
  tasks:
    - name: Copy and Execute the script
      script: /path/to/script.sh -arg1 -arg2
```
> See: [Ansible - Run A Local Script On Remote Server](https://blog.programster.org/ansible-run-a-local-script-on-remote-server)

----

### Files

File modules are used to manage files and directories on remote hosts. Some examples of file modules are:

- [copy](https://docs.ansible.com/ansible/latest/modules/copy_module.html): Copy files to remote locations.
- [fetch](https://docs.ansible.com/ansible/latest/modules/fetch_module.html): Fetch files from remote nodes.
- [file](https://docs.ansible.com/ansible/latest/modules/file_module.html): Manage files and file properties.
- [find](https://docs.ansible.com/ansible/latest/modules/find_module.html): Return a list of files based on specific criteria.
- [lineinfile](https://docs.ansible.com/ansible/latest/modules/lineinfile_module.html): Manage lines in text files.
- [stat](https://docs.ansible.com/ansible/latest/modules/stat_module.html): Retrieve file or file system status.
- [synchronize](https://docs.ansible.com/ansible/latest/modules/synchronize_module.html): A wrapper around rsync to make common tasks in your playbooks quick and easy.
- [template](https://docs.ansible.com/ansible/latest/modules/template_module.html): Template a file out to a remote server.
- [unarchive](https://docs.ansible.com/ansible/latest/modules/unarchive_module.html): Unpacks an archive after (optionally) copying it from the local machine.
- [acl](https://docs.ansible.com/ansible/latest/modules/acl_module.html): Sets and retrieves file ACL information.

**lineinfile module**
Manage lines in text files. search for a line and replace it or add it if it is not present.

Example:
In the following example, we will add a DNS server to `/etc/resolv.conf` file. lineinfile module will search for the line `nameserver 10.1.125.10` and add it if it is not present. If the line is present, it will not be added again.

`/etc/resolv.conf` file before running the playbook:


```yaml
nameserver 10.1.125.1
nameserver 10.1.125.2
```
playbook:
```yaml
- name: Add DNS server to resolv.conf
  hosts: localhost
  tasks:
    - name: Add DNS server to resolv.conf
      lineinfile:
        path: /etc/resolv.conf
        line: 'nameserver 10.1.125.10'
```
`/etc/resolv.conf` file after running the playbook:

```yaml
nameserver 10.1.125.1
nameserver 10.1.125.2
nameserver 10.1.125.10
```

if we run the playbook again, the file will not be changed because the line is already present.


### Database

database modules are used to manage databases. Some examples of database modules are:

- [mysql_db](https://docs.ansible.com/ansible/latest/modules/mysql_db_module.html): Add or remove MySQL databases from a remote host.
- [mysql_user](https://docs.ansible.com/ansible/latest/modules/mysql_user_module.html): Adds or removes a user from a MySQL database.
- [postgresql_db](https://docs.ansible.com/ansible/latest/modules/postgresql_db_module.html): Add or remove PostgreSQL databases from a remote host.

### Cloud

Cloud modules are used to manage cloud resources. Some examples of cloud modules are:

- [aws](https://docs.ansible.com/ansible/latest/collections/community/aws/index.html): Manage AWS resources.
- [docker](https://docs.ansible.com/ansible/latest/collections/community/docker/index.html): Manage the life cycle of Docker containers.
- [digital_ocean](https://docs.ansible.com/ansible/latest/collections/community/digital_ocean/index.html): Manage DigitalOcean Droplets and resources.

