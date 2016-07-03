#cài đặt php

```sh
#!bin/ansible-playbook
  - name : cai dat php
    hosts: webservers
    sudo: yes
    tasks:
     - name: install php5
	   apt: name={{items}}
	   with_items:
	    php5
	    libapache2-mod-php5
	    php5-mcrypt
```