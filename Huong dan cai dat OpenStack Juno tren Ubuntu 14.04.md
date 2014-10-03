# Huong dan cai dat OpenStack Juno tren Ubuntu 14.04

###
Chuẩn bị máy ảo trên môi trường Vmware Workstation hoặc Virtual Box. Trong hướng dẫn này sẽ thực hiện trên VMware Workstation

Cấu hình như hình dưới

#### Cài đặt các gói chuẩn bị
#### Thiết lập IP, hostname ...
Cấu hình IP như sau:

Dùng lệnh `sh vi /etc/network/interfaces` để thiết lập IP. Lưu ý thứ tự NICs phải đúng y/c. 

#### Cấu hình NTP
- Cài đặt gói NTP
```sh
apt-get update -y
apt-get install ntp -y
```
- Sửa file cấu hình /etc/ntp.conf. Comment dòng server ntp.ubuntu.com và chèn ngay dưới đó các dòng. 

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
- Kết quả sẽ như hình dưới
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


