#Step 1: How to Setup Server (DigitalOcean Droplet with Rocky Linux 10)

This is a tutorial on how to setup a secure server on Digital Ocean with Rocky Linux 10, and the core packages for additional integration tasks.
--
## 🚀 1. Create DigitalOcean Droplet

1. Log in to DigitalOcean.

2.Click on Droplets → Create Droplet.

3.Select the following configuration:

-Image: Rocky Linux 10 (x64)

-Plan: Simplest (lowest-priced is fine, such as $5/month)

-Location: Nearest you (eg Bangalore / Singapore)

-Authentication: Add SSH Key (preferred) or provide root password.

Click Create Droplet.

5.After the server is provisioned, record its IP address.
--
## 🔑 2. Secure Server Access

1.Access your server through SSH:
 ```bash
ssh root@your_server_ip
dnf update -y
adduser intern
passwd intern
usermod -aG wheel intern
firewall-cmd --permanent --add-service=ssh
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --reload
nano /etc/ssh/sshd_config
systemctl restart sshd
# Apache Web Server
dnf install httpd -y
systemctl enable httpd
systemctl start httpd

# MariaDB Database
dnf install mariadb-server -y
systemctl enable mariadb
systemctl start mariadb
mysql_secure_installation

# PHP 8.2
dnf module reset php -y
dnf module enable php:8.2 -y
dnf install php php-cli php-mysqlnd php-xml php-gd php-mbstring -y
systemctl restart httpd

# Python 3.11
dnf install python3.11 python3.11-pip -y
httpd -v        # Apache version
mysql -V        # MariaDB version
php -v          # PHP version
python3.11 -V   # Python version

