##Cài đặt Mysql-client

```sh
#!bin/ansible-playbook
- name : cai dat mysql-client
    hosts: webservers
    sudo: yes
    tasks:
     - name: install mysql client
       apt: name=mysql-client state=present
     - name: config
       lineinfile:
        dest=/etc/mysql/my.cnf
        regexp="bind-address            = (.)+127.0.0.1"
        line="bind-address            = 10.10.10.40
"
```