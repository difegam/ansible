# Handlers

In Ansible, handlers are a way to trigger specific tasks only when a change occurs in the system's state. Handlers are typically used to perform actions like restarting a service or executing other tasks after a configuration change has been made. They are defined in Ansible playbooks and are executed only if a task that notifies the handler has made changes to the system.

- Tasks triggered by events/notification
- Handlers are defined in playbooks, executed when notified by tasks
- Manage actions based on system state/configuration changes

## Example - Handlers

```yaml
- name: deploy application
  host: app_servers
  tasks:
    - name: Copy application Code
      copy:
        src: /tmp/app_code/
        dest: /opt/app/
      notify:
        - restart application

  handlers:
    - name: restart application
      service:
        name: application_service
        state: restarted
```
