#Triển khai Wordpress theo cây thư mục chuẩn của Ansible.

```sh
Bố cục của một chương trình Ansible chuẩn :
```

```sh
Ansible/
|---- files
|
| ---- group_vars
|      |----all
|----- roles
|      |---- web
|      |     |---- handlers
|      |     |     |---- main.yml
|      |     |---- tasks
|      |           |---- main.yml
|      |---- db
|            |---- handlers
|            |     |---- main.yml
|            |---- tasks
|                  |---- main.yml
|
|---- hosts
|
|---- wp.yml
|
|---- templates
```

- Trong cây thư mục chuẩn này chúng ta có thể đơn giản hóa được với các chương trình CM lớn , một số trường hợp có thể nhắc đến như sau :
 <ul>
  <li>Trong một chương trình CM chúng ta có nhiều biến lặp đi, lặp lại với nhau để tránh việc nhầm lẫn cũng như khai báo quá nhiều lần chúng ta chỉ cần khai báo 1 lần trong `group_vars`</li>
  <li>Một trương trình triển khai trên nhiều host thì chúng ta lại phải chạy số lần bằng với số host cần đến cho 1 chương trình, như thế sẽ mất thời gian ngồi đợi hết host này để tiếp tục triển khai trên host khác . Như thế sẽ không đúng với mục tiêu của các chương trình CM khi viết ra (chỉ cần ấn sau đó đi đâu đó làm ly cafe và đợi), với cây thư mục như này chúng ta có thể thực hiện một chương trình như thế chỉ bằng 1 thao tác.</li>
  <li>Tiện lợi trong cấu hình và chỉnh sửa lỗi vì các file cấu hình và các tác vụ được phân chia rõ ràng chứ không gồm trong 1 file.</li>
 </ul>

- Chúng ta cùng xem qua một chút về các file m thư mục và chức năng của nó :
 <ul>
  <li>files : đây là thư mục chứa tất cả các files mà chúng ta cần cho chương trình ansible để chúng ta coppy vào các máy client.</li>
  <li>group_vars/all : đây là file chứa tất cả các biến trong chương trình ansible.</li>
  <li>roles : Đây là thư mục chính chứa các file cấu hình cũng như triển khai mà chương trình ansible cần để thực hiện, có thể gọi đây là file điều khiển trung tâm của chương trình. Ở chương trình triển khai wordpress mình triển khai trên 2 host (một host làm web-server một web db) do đó trong đây chúng ta có 2 thư mục là web và db (lời khuyên : đối với các chương trình cần triển khai trên nhiều node thì mỗi node chúng ta nên tạo một thư mục chuyên cho node đó như trong ví dụ wp này.). Trong 2 thư mục này chúng ta có thể thấy 2 thư mục là handlers và tasks, trong 2 thư mục này lại đều có file main.yml , đây là 2 file mà chúng ta sử dụng cú pháp YAMl để điều khiển chương trình. Với thư mục task trong đây sẽ bao gồm các tác vụ như cài đặt, sửa file cấu hình còn với handlers sẽ thực hiện các tác vụ liên quan đến xử lý như restart service hay stop service.</li>
  <li>hosts : Đây là file cấu hình iventory (host) để máy chủ có thể nhận diện được tác vụ này sẽ được thực hiện trên những hosts nào, file host này sẽ quyết định vị trí file cấu hình mà ansible phải thực hiện.</li>
  <li>wp.yml : file triển khai điều khiển, nó sẽ gọi các tác vụ trong roles để thực hiện.</li>
  <li>templates: đây là file được viết bằng jinja2 , nếu như chúng ta có một file cấu hình hệ thống khác với cấu hình mặc định thì chúng ta nên làm 1 file templates và thay thế file cấu hình cũ không nên dùng regex để thay thế như thế sẽ mất rất nhiều thời gian cũng như các rủi ro về lỗi và cú pháp.</li>
 </ul>

```sh
Triển khai wordpress theo folder tree stardard:
```

- Mô hình thực hiện như sau :

```sh
                                        |------Client1 (Ubuntu 14.04) - Webserver (10.10.10.20)
                                        |
Server (Ubuntu 14.04) (10.10.10.10) ----|
                                        |
                                        |------Client2 (Ubuntu 14.04) - Database server (10.10.10.30)
```

- Trước tiên tại server chúng ta tiến hành Cài đặt Ansible để remote :

```sh
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update
sudo apt-get install -y ansible
```

- Chúng ta thiết lập file hosts `/etc/ansible/hosts` như sau :

```sh
[Webserver]
10.10.10.20
[dbserver]
10.10.10.30
```

- Sau đó tại server chúng ta tạo Key SSH :

```sh
ssh-keygen -t rsa -b 4096
```

- Sao chép key sang các client :

```sh
ssh-copy-id root@10.10.10.20
ssh-copy-id root@10.10.10.30
```

- Tiếp theo đó chúng ta tạo thư mục group_vars và file all :

```sh
mkdir group_vars
cd group_vars
vi all
```

- tạo roles và các thư mục cũng như file bên trong :

Tạo thư mục roles:

```sh
mkdir roles
cd roles
```

Tại đây chúng ta cần tại 2 thư mục web và db :

```sh
mkdir web
mkdir db
```

Sau đó chúng ta tạo 1 thư mục handlers và tasks cho web và db :

```sh
cd web
mkdir handlers
mkdir tasks
cd ..
cd db
mkdir handlers
mkdir tasks
```

Cài đặt cấu hình cho web và db (handlers và tasks của web và db).

Tạo task cho web :

```sh
vi /etc/ansible/roles/web/tasks/main.yml
```

Sau đó coppy đoạn YAML sau :

```sh
---
- name: INSTALL APACHE2
  apt: name=apache2 update_cache=yes
- name: CONFIG VIRTUAL HOST WordPress
  lineinfile:
    dest=/etc/apache2/sites-available/000-default.conf
    regexp="DocumentRoot /var/www/html"
    line="DocumentRoot /var/www/wordpress"
- name: INSTALL PHP5
  apt: name={{item}} update_cache=yes
  with_items:
    - php5
    - libapache2-mod-php5
    - php5-mcrypt
    - php5-mysqlnd-ms
- name: DOWNLOAD WordPress
  get_url:
    url=https://wordpress.org/latest.tar.gz
    dest=/tmp/wordpress.tar.gz
    validate_certs=no
- name: EXTRACT WordPress
  unarchive:
    src=/tmp/wordpress.tar.gz
    dest=/var/www/
    copy=no
- name: COPY FILE CONFIG Wordpress
  command: mv /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php
- name: FILE CONFIG Wordpress
  lineinfile:
    dest=/var/www/wordpress/wp-config.php
    regexp="{{ item.regexp }}"
    line="{{ item.line }}"
  with_items:
    - {'regexp': "define\\('DB_NAME', '(.)+'\\);", 'line': "define('DB_NAME', 'wordpress');"}
    - {'regexp': "define\\('DB_USER', '(.)+'\\);", 'line': "define('DB_USER', 'wordpress');"}
    - {'regexp': "define\\('DB_PASSWORD', '(.)+'\\);", 'line': "define('DB_PASSWORD', 'wordpress');"}
    - {'regexp': "define\\('DB_HOST', '(.)+'\\);", 'line': "define('DB_HOST', '10.10.10.30');"}
  notify: RESTART APACHE2
```

Tạo handlers cho web :

```sh
vi /etc/ansible/roles/web/handlers/main.yml
```

Sau đó coppy đoạn YAML sau vào file :

```sh
---
#cac handlers:
- name: RESTART APACHE2
  service: name=apache2 state=restarted
```

Tạo tasks cho db :

```sh
vi /etc/ansible/roles/db/tasks/main.yml
```

Sau đó coppy đoạn YAML sau vào file :

```sh
---
- name: INSTALL MYSQL
  apt: name={{item}} update_cache=yes
  with_items:
    - mysql-server
    - python-mysqldb
- name: CREATE DATABASE WordPress
  mysql_db: name=wordpress
- name: CREATE USER FOR DATABASE WordPress
  mysql_user: name=wordpress password=wordpress priv=wordpress.*:ALL host=10.10.10.20
- name: CONFIG MYSQL For Remote DB
  lineinfile:
    dest=/etc/mysql/my.cnf
    regexp="bind-address"
    line="bind-address = 0.0.0.0"
  notify: RESTART MYSQL Server
```

Tạo handlers cho db :

```sh
vi /etc/ansible/roles/db/handlers/main.yml
```

Sau đó coppy đoạn YAML sau vào file :

```sh
---
- name: RESTART MYSQL Server
  service: name=mysql state=restarted
```

- Chỉnh sửa file hosts :

```sh
vi /etc/ansible/hosts
```

Sau đó chỉnh sửa đoạn code sau trong file :

```sh
[webservers]
10.10.10.20
[dbservers]
10.10.10.30
```

- Thêm file wp.yml

```sh
vi /etc/ansible/wp.yml
```

Sau đó coppy đoạn YAMl sau vào file :

```sh
---
- name: cai dat wp
  hosts: webservers
  user: root
  roles:
    - web

- name: cai dat db
  hosts: dbservers
  user: root
  roles:
    - db
```

- Bây giờ chúng ta thực hiện chạy file playbook wp.yml rồi đi pha 1 ly cafe ngồi đợi thôi .

```sh
ansible-playbook wp.yml
```

- Nếu thành công chúng ta sẽ nhận được những dòng thông báo như sau :

```sh
root@datpt:/etc/ansible# ansible-playbook wp.yml 

PLAY [cai dat wp] **************************************************************

TASK [setup] *******************************************************************
ok: [10.10.10.20]

TASK [web : INSTALL APACHE2] ***************************************************
changed: [10.10.10.20]

TASK [web : CONFIG VIRTUAL HOST WordPress] *************************************
changed: [10.10.10.20]

TASK [web : INSTALL PHP5] ******************************************************
changed: [10.10.10.20] => (item=[u'php5', u'libapache2-mod-php5', u'php5-mcrypt', u'php5-mysqlnd-ms'])

TASK [web : DOWNLOAD WordPress] ************************************************
changed: [10.10.10.20]

TASK [web : EXTRACT WordPress] *************************************************
changed: [10.10.10.20]

TASK [web : COPY FILE CONFIG Wordpress] ****************************************
changed: [10.10.10.20]

TASK [web : FILE CONFIG Wordpress] *********************************************
changed: [10.10.10.20] => (item={u'regexp': u"define\\('DB_NAME', '(.)+'\\);", u'line': u"define('DB_NAME', 'wordpress');"})
changed: [10.10.10.20] => (item={u'regexp': u"define\\('DB_USER', '(.)+'\\);", u'line': u"define('DB_USER', 'wordpress');"})
changed: [10.10.10.20] => (item={u'regexp': u"define\\('DB_PASSWORD', '(.)+'\\);", u'line': u"define('DB_PASSWORD', 'wordpress');"})
changed: [10.10.10.20] => (item={u'regexp': u"define\\('DB_HOST', '(.)+'\\);", u'line': u"define('DB_HOST', '10.10.10.30');"})

RUNNING HANDLER [web : RESTART APACHE2] ****************************************
changed: [10.10.10.20]

PLAY [cai dat db] **************************************************************

TASK [setup] *******************************************************************
ok: [10.10.10.30]

TASK [db : INSTALL MYSQL] ******************************************************
changed: [10.10.10.30] => (item=[u'mysql-server', u'python-mysqldb'])

TASK [db : CREATE DATABASE WordPress] ******************************************
changed: [10.10.10.30]

TASK [db : CREATE USER FOR DATABASE WordPress] *********************************
changed: [10.10.10.30]

TASK [db : CONFIG MYSQL For Remote DB] *****************************************
changed: [10.10.10.30]

RUNNING HANDLER [db : RESTART MYSQL Server] ************************************
changed: [10.10.10.30]

PLAY RECAP *********************************************************************
10.10.10.20                : ok=9    changed=8    unreachable=0    failed=0   
10.10.10.30                : ok=6    changed=5    unreachable=0    failed=0   

root@datpt:/etc/ansible# 


```

- Và đây là kết quả chúng ta thu được :

![scr](http://i.imgur.com/jSrpt3C.png)

#Finish !!!