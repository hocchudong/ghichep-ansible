##install mysql-server

```sh
#!bin/ansible-playbook
  - name : cai dat mysql-sever
    hosts: dbservers
    sudo: yes
    tasks:
     - name: install mysql server
       apt: name={{item}} state=present
       with_items:
        mysql-server
        python-mysqldb
 #tao db va user
     - mysql_db: name=wptest state=present
     - mysql_user:
        name=wptest
        password=wptest
        priv=wptest.*:ALL
        state=present
        host=10.10.10.60
     - name: config
       lineinfile:
        dest=/etc/mysql/my.cnf
        regexp="bind-address            = (.)+127.0.0.1"
        line="bind-address            = 10.10.10.40
"      
```