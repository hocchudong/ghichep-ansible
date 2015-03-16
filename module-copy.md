# Giới thiệu về module copy trong Ansible
- Dùng để copy file từ máy điều khiển (server) tới các máy ở xa (các node), ngược lại với module `fetch`

- Cách dùng
```sh
-m copy -a 'src=duong_dan_file_tren_server dest=duong_dan_tuyet_doi_may_o_xa'
```