## Các ghi chép về `Playbook` trong `Ansible`
- Task là một việc hay yêu cầu được thực hiện trong Ansible. (#Chỗ này chưa rõ ý lắm)
- Mỗi `task` trong ansible là một play
- Nhiều play kết hợp lại gọi là `Playbook`

### Cú pháp dùng
- Dùng với lệnh
```sh
ansible-playbook file_yaml.yml
```

### Các ghi chú
- Sử dụng Playbook với file host cụ thể, sử dụng tùy chọn `-i`
```sh
ansible-playbook  -i  duongdan/file_host  duongdan/file_yaml.yml
```
Minh họa
![ansible-ping](images/ansible-playbook1.png)

- Sử dụng dụng tùy chọn `--list-hosts` trong `playbook` để kiểm tra các host được áp dụng.
Minh họa
![ansible-ping](images/ansible-playbook2.png)