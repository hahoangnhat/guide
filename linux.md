# Setup môi trường Linux
Hướng dẫn cài đặt môi trường phát triển Linux

## 1. Cập nhật Linux packages
Thực thi các lệnh sau
```
$ sudo apt update
$ sudo apt upgrade
```

## 2. Install Java
Kiểm tra máy đã cài đặt Java hay chưa bằng lệnh sau
```
$ which java
```
> **Note:** Nếu lệnh không output gì, có nghĩa là máy chưa cài đặt

Nếu máy chưa cài đặt, thực thi lệnh sau
```
$ sudo apt install default-jdk
```
> **Note:** Các project hiện tại đang sử dụng Java 11 

Kiểm tra version của Java
```
$ java --version
```

## 3. Install Jetty server
Install command
```
$ sudo apt install jetty9
```

Sau khi cài đặt, Jetty sẽ tự động chạy ngầm, sử dụng lệnh sau để check quá trình cài đặt chính xác
```
$ apt show jetty9
```

Tại thời điểm này, kết quả check có thể giống như sau
```
Package: jetty9
Version: 9.4.15-1~18.04.1ubuntu1
Priority: optional
Section: universe/java
Origin: Ubuntu
```
Bạn có thể truy cập Jetty 9 application bằng URL: **http://<your_ip_address>:8080/**
Jetty mặc định chạy ở port 8080

### 3.1 Managing the Jetty 9 Service
Enable the Jetty 9 at boot time
```
$ sudo systemctl enable jetty9
```

Start Jetty 9 service
```
$ sudo systemctl start jetty9
```

Restart Jetty 9
```
$ sudo systemctl restart jetty9
```

Stop Jetty 9
```
$ sudo systemctl stop jetty9
```

Check the service status
```
$ sudo systemctl status jetty9
```

## 4. Install Reverse Proxy in Apache
Reverse proxy sẽ điều phối request tới các server đang chạy trong hệ thống, port request và port forward đều có thể tự chỉ định

Install commanđ
```
$ sudo apt-get install apache2
```

Enable two Apache modules: proxy and proxy_http
```
$ sudo a2enmod proxy
$ sudo a2enmod proxy_http
```

Sau khi enable 2 modules trên cần restart Apache
```
$ sudo systemctl restart apache2
```

Tạo file config reverse proxy tại folder /etc/apache2/sites-available/<your_domain.com>.conf
```
<VirtualHost *:80>
  ServerName <your_domain>.com
  ServerAlias www.<your_domain>.com
  ProxyRequests off
  ProxyPass / http://127.0.0.1:8080/
  ProxyPassReverse / http://127.0.0.1:8080/
</VirtualHost>
<VirtualHost *:81>
  ServerName <your_domain_1>.com
  ServerAlias www.<your_domain_1>.com
  ProxyRequests off
  ProxyPass / http://127.0.0.1:8081/
  ProxyPassReverse / http://127.0.0.1:8081/
</VirtualHost>
```

Enable the ‘your-domain.com.conf’ Apache, and restart Apache để apply config
```
$ sudo a2ensite <your_domain.com>.conf
$ sudo systemctl restart apache2
```
> **Note:** Thay đổi <your_domain> với domain của bạn

Lúc này bạn có thể truy cập Jetty 9 application bằng URL: https://<your_domain>.com qua port 80, 81,... (tùy theo file conf bạn setup)

## 5. Install Jenkins
Đây là repository Debian của Jenkins để tự động cài đặt và nâng cấp. Để sử dụng repository này, trước tiên hãy thêm khóa vào hệ thống của bạn:
```
$ curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

Sau đó add Jenkins apt repository entry:
```
$ echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Update local package index, sau đó install Jenkins:
```
$ sudo apt-get update
$ sudo apt-get install fontconfig openjdk-11-jre
$ sudo apt-get install jenkins
```

Thay đổi port của jenkins tại file /lib/systemd/system/jenkins.service
> Environment="JENKINS_PORT=8080"

## 6. Generating the SSH Key
Generate the pair of keys
```
$ ssh-keygen -t rsa
```

Copy your ssh key to remote server
```
$ ssh-copy-id user@serverip
```

## 7. Giving Full Sudo Access to a User

### Solution 1: Add this command at the end of Sudoers File
```
%<user> ALL=(ALL:ALL) NOPASSWD: ALL
```

### Solution 2: Adding the User to the Sudo Group
```
$ sudo usermod -aG sudo <user>
```

> **Note:** Remember to replace \<user\> with your actual user's name

## 8. Change default editor
```
$ sudo update-alternatives --config editor
```
