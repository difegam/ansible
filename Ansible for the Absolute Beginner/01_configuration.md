# Ansible Configuration files

## Global ansible.cfg

`/etc/ansible/ansible.cfg`

## User ansible.cfg

`~/.ansible.cfg`

# Ansible Configuration variables

command line variables

```bash
ANSIBLE_GATHERING=explicit  ansible-playbook playbooks.yml
```

environment variables

```bash
export ANSIBLE_GATHERING=explicit
ansible-playbook playbooks.yml
```

persistent variables

```yaml
# /opt/web-playbooks/ansible.cfg
gathering = explicit
```

## View configuration

List all configurations

```bash
ansible-config list
```

Shows the current configuration file

```bash
ansible-config view
```

Show the currents settings

```bash
ansible-config dump
```

Show the changed settings

```bash
ansible-config dump --only-changed
```

```bash
export ANSIBLE_GATHERING=explicit
ansible-config dump | grep GATHERING

# output

$ GATHERING (/home/ansible/.ansible.cfg) = explicit
```
