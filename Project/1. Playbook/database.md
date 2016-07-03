#Cài đặt và tạo DB

```sh
#!bin/ansible-playbook
  - name : cai dat mysql
    hosts: webservers
    sudo: yes
    tasks:
	 - name: install mysql
       apt: name={{item}}
	   with_items:
        mysql-server
        python-mysqldb
     - name: Tao database wordpress
       mysql_db: name=wordpress state=present
	   mysql_user: name=wordpress password=wordpress priv=wordpress.*:ALL state=present
```