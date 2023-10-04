# Playbook

In ansible a playbook is a list of plays. A play is a list of tasks. A task is a call to an ansible module. A module is a reusable, standalone script that Ansible runs on your behalf, either locally or remotely. Modules can be written in any language and are found in the Ansible code base or on the Ansible Galaxy website.

## Play properties

- `name`: Name of the play
- `hosts`: Hosts to run the play on
- `gather_facts`: Gather facts about the hosts in your inventory
- `vars`: Variables that can be used in the play
- `tasks`: List of tasks to run
- `handlers`: List of handlers to run
- `pre_tasks`: List of tasks to run before the tasks in the play
- `post_tasks`: List of tasks to run after the tasks in the play
- `roles`: List of roles to run

see more: [Play properties](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#play-properties)

Examples:
`/home/bob/playbooks/01_playbook.yml`
This playbook runs on all hosts in the inventory. It prints a hello world message.

```yaml
- name: Print hello world
  hosts: all
  gather_facts: no
  tasks:
    - debug:
        msg: Hello world
```

## Ansible modules

Ansible modules are small, reusable units of code that perform specific tasks on remote hosts. These modules are an essential part of Ansible, a popular open-source automation tool used for configuration management, application deployment, and task automation across multiple servers. Ansible modules enable you to define and manage the desired state of your infrastructure, making it easy to automate various IT operations.

### Types of Modules:

**Core Modules**: These modules come pre-packaged with Ansible and cover a wide range of common tasks like package installation, service management, file manipulation, and more.

**Custom Modules**: Users can create their own custom modules to extend Ansible's functionality to suit their specific needs. Custom modules are typically written in Python or another scripting language.

**Examples of Core Modules**: Some commonly used core modules include:

- `apt`, `yum`, and `dnf`: For package management on Linux systems.
- `service`: To manage services.
- `copy`, `template`, and `file`: For working with files.
- `command`, `shell`, and `script`: For running commands or scripts.
- `user` and `group`: To manage users and groups.
- `cron`: For managing cron jobs.
- `git`: To interact with Git repositories.

You run the following command to see all modules:

```bash
ansible-doc -l
```

see more: [Ansible modules](https://docs.ansible.com/ansible/latest/user_guide/modules_intro.html#ansible-modules)

## Run playbook

```bash
ansible-playbook -i inventory 01_playbook.yml
```

## Verify playbook

Ansible has two modes to verify a playbook: check mode and diff mode.

check mode: `--check` or `-C`
diff mode: `--diff` or `-D`

## Check mode

Check mode is a way to test changes without actually applying them. It is useful for tasks that change things on the target hosts and you want to preview the changes before applying them.

```bash
ansible-playbook -i inventory 01_playbook.yml --check
```

## Diff mode

Diff mode is a way to preview changes that Ansible will make on the target hosts. It is useful for tasks that change things on the target hosts and you want to preview the changes before applying them.

```bash
ansible-playbook -i inventory 01_playbook.yml --diff
```

## Syntax check

Ansible has a syntax checker that will check your playbook for syntax errors.

Syntax check: `--syntax-check` or `-s`

```bash
ansible-playbook -i inventory 01_playbook.yml --syntax-check
```

## Ansible Lint

Ansible Lint is a command-line tool for linting playbooks. It checks your playbooks for best practices and common mistakes.

```bash
ansible-lint 01_playbook.yml
```
