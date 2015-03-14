# ghichep-ansible
Ghi chép về ansible


## Mục lục 

### Lịch sử ghi chép
- 14/03/2015: Tạo tài liệu | congto

### Ghi chép

### Vai trò - Chức năng
- Dùng để configuration management
- Là provisioning tool 

### Cách cài đặt
- Mô hình
```sh
                        |------Client1(Ubuntu 12.04)
                        |
SRV(Ubuntu14.04)--------|------Client2(Ubuntu 14.04)
                        |
                        |------Client3(CentOS 6.5)
```
- Môi trường
```python
python test
Ubuntu 14.04-2 64bit
```
- Lệnh cài đặt
```sh
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update
sudo apt-get install -y ansible
```

- Phiên bản của Ansible hiện tại (14.03.2015)
```sh
root@u14:~# ansible --version
ansible 1.8.4
  configured module search path = None
root@u14:~#
```

### Tham khảo tài liệu
[1] https://serversforhackers.com/an-ansible-tutorial
