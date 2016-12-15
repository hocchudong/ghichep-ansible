#Cấu hình tự động cài đặt Wordpress sử dụng Ansible.

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

- Tại thư mục `/etc/ansible` tạo thư mục `Wordpress`

```sh
mkdir wordpress
```

- Vào thư mục wordpress:

```sh
cd wordpress
```

- tạo 2 playbook [web.yml](https://github.com/datkk06/ghichep-ansible/blob/master/Project/wordpress-ansible/web.yml) và [db.yml](https://github.com/datkk06/ghichep-ansible/blob/master/Project/wordpress-ansible/db.yml)

- sau đó chạy 2 playbook :

```sh
ansible-playbook web.yml
ansible-playbook db.yml
```

- Kết quả thu được :

![scr1](http://i.imgur.com/E4XX1lU.png)
![scr2](http://i.imgur.com/mlgO7Li.png)

- Vào web server để kiểm tra :

![scr3](http://i.imgur.com/rwcBQ31.png)