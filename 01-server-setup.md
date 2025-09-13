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
[updatepage](screenshots'/update.png)
[newuser](screenshots'/newuser.png)
[firewallsetup](screenshots'/firewallsetup.png)
[corepackage](screenshots'/installedcorepackages.png)
[corepackages2](screenshots'/installedcorepackages2.png)



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

---

### 2️⃣ Start Day 2 text normally
Then, write your Day 2 letter-style text **without any backticks**:

```markdown
## Day 2 – Server Setup

---

## 0) Verify VM & Internet

Run:

```bash
hostnamectl
ip addr show
ping -c 4 8.8.8.8
ping -c 4 google.com
free -h
df -h /
```

**Why:** Confirms network, disk, memory — useful evidence for docs.

**Screenshot #1:** 
[for_verifyVM](screenshots'/2day1.png)
[for_verifyVM2](screenshots'/2day2.png)

---

## 1) Ensure package repos & clean cache

Run:

```bash
sudo dnf clean all
sudo rm -rf /var/cache/dnf
sudo dnf makecache
```

**Why:** Guarantees fresh metadata (fixes stale/mirror problems).

*No screenshot required unless errors occur.*

---

## 2) Add EPEL & Remi repos

```bash
sudo dnf install -y epel-release
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-10.rpm
sudo dnf module enable -y php:remi-8.3
```

**Why:** Remi provides up-to-date PHP builds used by Drupal/PHP apps.

**Screenshot #2:
[epelremi](screenshots'/2day3.png)
[epelremi1](screenshots'/2day4.png)
---

## 3) Install Apache, PHP, PHP-FPM, MariaDB, Python, Composer, and Tools

```bash
sudo dnf install -y httpd php php-cli php-mysqlnd php-gd php-xml php-mbstring php-json php-fpm \
mariadb-server python3 python3-pip composer unzip wget git
```

**Why:** Installs runtime stacks for Drupal (PHP), Django (Python), and MariaDB.

**Screenshot #3:** 
[InstallApache,PHP,PHP-FPM,MariaDB,Python,Composer,andTools1](screenshots'/2day8.png)
[InstallApache,PHP,PHP-FPM,MariaDB,Python,Composer,andTools2](screenshots'/2day9.png)
[InstallApache,PHP,PHP-FPM,MariaDB,Python,Composer,andTools3](screenshots'/2day10.png)
---

## 4) Enable & Start Services

```bash
sudo systemctl enable --now httpd
sudo systemctl enable --now php-fpm
sudo systemctl enable --now mariadb
```

**Why:** Makes services start now and on boot.

**Screenshot #4:** `screenshots/day2-services.png`
*(Show `systemctl status httpd` and `systemctl status mariadb` — combined or separate)*

---

## 5) Quick Verification of Runtime Versions

```bash
httpd -v
php -v
mysql -V
python3 -V
composer -V
```

**Why:** Proves exact versions installed.

**Screenshot #5:** `screenshots/day2-versions.png`
*(One image showing outputs or separate files named accordingly)*

---

## 6) Secure MariaDB

```bash
sudo mysql_secure_installation
```

Follow prompts:

* Set root password (if not set during install)
* Remove anonymous users
* Disallow remote root login
* Remove test DB
* Reload privilege tables: yes

**Why:** Basic hardening of the database.

**Screenshot #6:** `screenshots/day2-mysql-secure.png`
*(Screenshot of the final success message; blur any passwords)*

---

## 7) Test Apache Default Page

From VM or host (if networking/NAT configured):

```bash
curl -I http://localhost
# or
curl http://127.0.0.1 | head -n 5
```

You should get HTTP 200 or the default Apache page.

**Screenshot #7:** `screenshots/day2-apache-test.png`
*(Capture curl output or open VM browser showing Apache default page)*

---

## Notes

* Optional swap creation and database creation were **skipped**.
* SELinux adjustments were **not needed** this session.

---

## End of Day 2





