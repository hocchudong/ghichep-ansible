#ANSIBLE NOTE
Một số chú ý về ansible mà bản thân tác giả (cosy294) đã rút ra được, trong quá trình triển khai. :v

#1. Về cú pháp YAML.
- YAML bắt đầu bằng `---` và kết thúc bằng `...`
- YAML sử dụng indent là 2 space. Chú ý về space, chỉ cần thừa hoặc thiếu 1 space là có thể sai ngay.
- Text editor có thể là:
  - Notepad ++.
  - Sublime Text.
  - Atom.
  - ....
- Đối với Notepad++, bạn cần chuyển đổi `TABs to spaces` để có thể dễ dàng chỉnh sửa file yaml với phím TAB. Nếu không, thì không nên sử dụng phím TAB để trong yaml.

  Để thực hiện, vào `Settings -> Preferences -> Language Menu/Tab Settings`, rồi chọn như hình dưới đây.
  ![](http://i.imgur.com/aNrTJs1.png)

- Để check cú pháp YAML, mình có 2 cách sau đây:
  - 1. Rất đơn giản, truy cập trang web http://www.yamllint.com/, post đoạn yaml cần check và nhấn `GO`. Trang web sẽ trả về kết quả cho bạn.
  - 2. Sử dụng Plugin Pretty YAML với Sublime Text. Chi tiết tại đây: https://packagecontrol.io/packages/Pretty%20YAML

#2. Về Ansible.
- Ansible hỗ trợ rất nhiều module để mình có thể viết playbooks. Chỉ cần search từ khóa + ansible là có thể tìm ra.
- Ansible Galaxy, một kho tàng các roles mà mọi người trên toàn thế giới có thể đóng góp. Bạn muốn triển khai một cái gì đó, hãy thử search trên Ansible Galaxy xem đã có chưa :))). `“Ansible Galaxy” can either refer to a website for sharing and downloading Ansible roles, or a command line tool for managing and creating roles` Chi tiết tại https://galaxy.ansible.com/
- Cấu trúc một playbook chuẩn
```sh
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
   group1                 # here we assign variables to particular groups
   group2                 # ""
host_vars/
   hostname1              # if systems need specific variables, put them here
   hostname2              # ""

library/                  # if any custom modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```
