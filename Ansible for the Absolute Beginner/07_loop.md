# Loops

In Ansible Loop is used to execute a task multiple times. It is used to iterate over a list of items, and execute a task once for each item in the list.

# with_items vs loop

`with_items` is the old way of looping over a list of items. The `loop` keyword is the new way of looping over a list of items. The `loop` keyword is more readable and easier to use than `with_items`.

## loop keyword

The `loop` keyword is used to loop over a list of items. The task will be executed once for each item in the list.

```yaml
- name: Create Users
  hosts: all
  tasks:
    - name: Create user
      user:
        name: "{{ item }}"
        state: present
      loop:
        - user1
        - user2
        - user3
```

### Example - Looping over a dictionary

```yaml
- name: Create Users
  hosts: all
  tasks:
    - name: Create user
      user:
        name: "{{ item.username }}"
        uid: "{{ item.uid }}"
        state: present
      loop:
        - username: user1
          uid: 1001
        - username: user2
          uid: 1002
        - username: user3
          uid: 1003
```

```yaml
- name: Creating users
  hosts: localhost
  tasks:
    - name: Create user
      user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        state: present
      loop:
        - { name: user1, uid: 1010 }
        - { name: user2, uid: 1011 }
        - { name: user3, uid: 1012 }
```

## Looping over a list

We can use the `with_items` statement to loop over a list of items. The task will be executed once for each item in the list.

```yaml
- name: Install applications
  yum:
    name: "{{ item }}"
    state: present
  with_items:
    - vim
    - sqlite
    - jq
```

# with directive

``

- with_items
- with_file
- with_url
- with_dict

# Until

until directive is used to wait for a condition to become true. The task will be executed until the condition is true.

```yaml
- name: Wait for port 22 to become open and respond
  wait_for:
    port: 22
    host:
    state: started
    delay: 10
    timeout: 60
    until: result is success
```



# Examples
## Example 1


```yaml
---
-  name: 'Print list of fruits'
   hosts: localhost
   vars:
     fruits:
       - Apple
       - Banana
       - Grapes
       - Orange
   tasks:
     - command: 'echo "{{ item }}"'
       loop: "{{ fruits }}"
```

```yaml
---
-  name: 'Print list of fruits'
   hosts: localhost
   vars:
     fruits:
       - Apple
       - Banana
       - Grapes
       - Orange
   tasks:
     - command: 'echo "{{ item }}"'
       with_items: '{{ fruits }}'
```
## Example 2

```yaml

---
- name: 'Install required packages'
  hosts: localhost
  become: yes
  vars:
    packages:
      - httpd
      - make
      - vim
  tasks:
    - yum:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"
```
```yaml
---
- name: 'Install required packages'
  hosts: localhost
  become: yes
  vars:
    packages:
      - httpd
      - make
      - vim
  tasks:
    - yum:
        name: '{{ item }}'
        state: present
      with_items: '{{ packages }}'
``````

## Example 3

Here’s an example playbook that demonstrates the use of with_file, with_url, and with_mongodb with directives:

```yaml
---
- name: Example playbook
  hosts: localhost
  become: true
  vars:
    # Define a variable for the MongoDB server URL
    mongodb_server: "mongodb://localhost:27017/"
  tasks:
    # Use with_file to copy contents of a file to remote server
    - name: Copy contents of a file to remote server
      copy:
        content: "{{ item }}"
        dest: "/tmp/{{ item }}"
      with_file:
        - "file1.txt"
        - "file2.txt"
      # The copy module copies the contents of each file to /tmp folder
      # on the remote server.

    # Use with_url to download a file from a URL
    - name: Download file from URL
      get_url:
        url: "{{ item }}"
        dest: "/tmp/"
      with_url:
        - "http://example.com/file1.txt"
        - "http://example.com/file2.txt"
      # The get_url module downloads the contents of each file to /tmp folder
      # on the local machine.

    # Use with_mongodb to execute a query on a MongoDB server
    - name: Execute MongoDB query
      mongodb_query:
        database: "mydb"
        collection: "mycollection"
        query: "{{ item }}"
      with_mongodb:
        - '{"name": "John"}'
        - '{"name": "Jane"}'
      # The mongodb_query module executes the query for each item in the loop,
      # using the defined MongoDB server URL and database/collection name.
```
__Ref: [Ansible Loop and with_directive](https://medium.com/@amankroy98/ansible-loop-and-with-direct-a191bf5bce19)__

___
> See more: __[Ansible Loops](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html)__

