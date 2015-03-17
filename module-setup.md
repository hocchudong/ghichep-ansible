# Cách sử dụng module setup
- Dùng để lấy thông tin về các host ở xa: hostname, ip ...

### Cách dùng
- Dùng với lệnh
```sh
Hiển thị tất cả các thông tin ở host từ xa

ansible all -m setup

hoặc dùng với tùy chọn filter để lọc ra các thông tin cần thiết, ví dụ dưới lọc ra thông tin về địa chỉ IP của host ở xa

ansible all -m setup -a 'filter=ansible_all_ipv4_addresses'

```

Minh họa:
![ansible-setup2](images/ansible-setup2.png)


- Dùng với playbook

