# Huong dan cai dat OpenStack Juno tren Ubuntu 14.04

###
Chuẩn bị máy ảo trên môi trường Vmware Workstation hoặc Virtual Box. Trong hướng dẫn này sẽ thực hiện trên VMware Workstation

Cấu hình như hình dưới

#### Cài đặt các gói chuẩn bị
#### Thiết lập IP, hostname ...
Cấu hình IP như sau:

Dùng lệnh `sudo vi /etc/network/interfaces` để thiết lập IP. Lưu ý thứ tự NICs phải đúng y/c. 

#### Cấu hình NTP
- Cài đặt gói NTP
```sh
apt-get update -y
apt-get install ntp -y
```
- Sửa file cấu hình `/etc/ntp.conf`. Comment dòng `server ntp.ubuntu.com` và chèn ngay dưới nó các dòng sau. 

```sh
server 0.vn.pool.ntp.org iburst
server 1.asia.pool.ntp.org iburst
server 2.asia.pool.ntp.org iburst
```
- Comment các dòng 
```sh
restrict -4 default kod notrap nomodify nopeer noquery
restrict -6 default kod notrap nomodify nopeer noquery
```
và thay bằng dòng
```sh
restrict -4 default kod notrap nomodify
restrict -6 default kod notrap nomodify
```

- Khởi động lại NTP và kiểm tra NTP bằng các lệnh sau:
```sh
service ntp restart 

ntpq -c peers
```
- Kết quả sẽ tương tự như bên dưới.
```sh
openstack:~# ntpq -c peers
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 mx1198.digipowe 118.102.5.136    3 u   55   64    1   28.257   57.882   0.000
 136.18.0.14.in- 133.243.238.243  2 u   55   64    1   30.917   26.831   0.000
 time.vng.vn     .INIT.          16 u    -   64    0    0.000    0.000   0.000
 89.218.2.66.sta .INIT.          16 u    -   64    0    0.000    0.000   0.000
*time1.isu.net.s .GPS.            1 u   41   64    1  425.034   16.341   0.273
 localhost       .INIT.          16 l    -   64    0    0.000    0.000   0.000
```

#### Cài đặt và cấu hình Database
- Trong tài liệu này sẽ sử dụng MariaDB (một nhánh của MySQL được hình thành sau khi MySQL được Oracle mua lại). Trong qúa trình cài bạn sẽ nhập mật khẩu cho  tài khoản `root` để đăng nhập vào MariaDB.
```sh
apt-get install mariadb-server python-mysqldb -y
```
- Sau khi cài đặt xong, dùng vi để sửa file `/etc/mysql/my.cnf`. Tìm dòng có chuỗi `bind-address` và sửa thành.
```sh
bind-address 0.0.0.0
```
- Chèn vào sau dòng `bind-address 0.0.0.0` vừa sửa ở trên đoạn cấu hình dưới đây.
```sh
default-storage-engine = innodb
innodb_file_per_table
collation-server = utf8_general_ci
init-connect = 'SET NAMES utf8'
character-set-server = utf8
```

- Khởi động lại mysql và kiểm tra 
```sh
openstack:~# mysql -u root -p
```
- Kết quả như dưới
```sh
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 28
Server version: 5.5.39-MariaDB-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2014, Oracle, Monty Program Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
MariaDB [(none)]>

```