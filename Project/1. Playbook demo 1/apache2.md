#Cài đặt apache và cấu hình

```sh
#!bin/ansible-playbook
  - name : cai dat apache 
    hosts: webservers
    sudo: yes
    tasks:
	 - name: install apache
	   apt: name=apache2 update_cache=yes state=present
     - name: config file
       lineinfile:
		dest=/etc/apache2/sites-available/000-default.conf
	    regexp="(.)+DocumentRoot /var/www/html"
	    line="DocumentRoot /var/www/wordpress"
     - name: restart apache
       service: name=apache2 state=restarted
```