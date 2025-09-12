# 01 - Server Setup (Rocky Linux 10 on VirtualBox)

**Goal:** Provision a secure Rocky Linux 10 VM for the internship tasks and install core packages that we will need later (web server, database, PHP, Python tools).  
**Environment used:** VirtualBox VM (Rocky Linux 10, 4GB RAM, 40GB disk, 2 vCPUs).

---

## 1. Summary of what I did (Day 1)
- Installed Rocky Linux 10 on VirtualBox.
- Created a non-root sudo user.  
- Disabled root SSH login.  
- Enabled and configured `firewalld`.  
- Installed core packages: Apache (httpd), MariaDB, PHP, Python, developer tools.
- Performed system update and basic verification.

---

## 2. Why these steps matter (short explanations)
- **Non-root user & disable root login:** improves security — attackers shouldn't be able to log in directly as `root`.  
- **firewalld + allow ports:** ensures only required services (SSH / HTTP / HTTPS / Keycloak port 8080) are reachable.  
- **dnf update / package installation:** keeps OS secure and installs the software the apps will need (web server, database, language runtimes).  
- **Enabling services:** makes services persistent after reboot (so the server remains functional).

---
## screenshots
![updatepage](update.png)
![newuser](newuser.png)
![firewallsetup](firewallsetup.png)
![corepackage](installedcorepackages.png)
![corepackages2](installedcorepackages2.png)



## 3. Commands executed (run one by one; explanations after each block)

### 3.1 Update package metadata & upgrade
```bash
sudo dnf clean all
sudo dnf makecache
sudo dnf update -y
# create user
sudo adduser projectuser
# set password (enter a secure password when prompted)
sudo passwd projectuser
# add to wheel group for sudo access
sudo usermod -aG wheel projectuser
sudo systemctl enable --now firewalld
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --permanent --add-port=8080/tcp   # Keycloak (dev/default)
sudo firewall-cmd --reload
# verify
sudo firewall-cmd --list-all
sudo dnf install -y epel-release
# install Remi repo for newer PHP if needed
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-10.rpm
sudo dnf module enable -y php:remi-8.3

# install main packages
sudo dnf install -y httpd php php-cli php-mysqlnd php-gd php-xml php-mbstring php-json php-fpm mariadb-server python3 python3-pip git wget unzip composer






