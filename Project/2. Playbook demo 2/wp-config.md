##tải và cấu hình wp

```sh
#!bin/ansible-playbook
  - name : cai dat WP 
    hosts: webservers
    sudo: yes
    tasks:
	 - name: tai wordpress
       get_url:
        url=https://w...content-available-to-author-only...s.org/latest.tar.gz 
        dest=/tmp/wordpress.tar.gz
        validate_certs=no 
     - name: Extract WordPress
       unarchive:
	    src=/tmp/wordpress.tar.gz
	    dest=/var/www/
	    copy=no 
     - name: copy file config
       command: mv /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php creates=/var/www/wordpress/wp-config.php
     - name: config file
       lineinfile:
        dest=/var/www/wordpress/wp-config.php
        regexp="{{ item.regexp }}"
        line="{{ item.line }}"
       with_items:
        - {'regexp': "define\\('DB_NAME', '(.)+'\\);", 'line': "define('DB_NAME', 'wptest');"}        
        - {'regexp': "define\\('DB_USER', '(.)+'\\);", 'line': "define('DB_USER', 'wptest');"}        
        - {'regexp': "define\\('DB_PASSWORD', '(.)+'\\);", 'line': "define('DB_PASSWORD', 'wptest');"}
```