# 02 – Keycloak Setup

This document explains the installation and configuration of **Keycloak** on the Rocky Linux VM.

---

## Installation Steps

### 1. Install Java 17
```bash
sudo dnf install -y java-17-openjdk
java -version
```
2. Download and Extract Keycloak
```bash
cd /opt
sudo curl -L -O https://github.com/keycloak/keycloak/releases/download/24.0.4/keycloak-24.0.4.zip
sudo dnf install -y unzip
sudo unzip keycloak-24.0.4.zip
sudo mv keycloak-24.0.4 keycloak
```

3. Create Keycloak User
```bash
Copy code
sudo useradd -r -s /sbin/nologin keycloak
sudo chown -R keycloak:keycloak /opt/keycloak
```
4. Start Keycloak in Development Mode
```bash
Copy code
sudo -u keycloak /opt/keycloak/bin/kc.sh start-dev --http-port=8080 --hostname=0.0.0.0
⚠️ Dev mode is used for testing only. Production requires HTTPS certificates.
  ```

5. Open Firewall Port
```bash
Copy code
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
```
6. Create Admin User
When prompted:

makefile
```Copy code
Username: admin
Password: ********
```
7. Access Admin Console
In browser:

cpp
```Copy code
http://<VM_IP>:8080
Login with the admin credentials.
```

Screenshots  
[screenshot1](screenshots'/3day1.png)  
[screenshot2](screenshots'/3day2.png)  
[screenshot3](screenshots'/3day3.png) 
[screenshot4](screenshots'/3day4.png) 
[screenshot5](screenshots'/3day5.png) 
[screenshot6](screenshots'/3day6.png) 
[screenshot7](screenshots'/3day7.png) 
[screenshot8](screenshots'/3day8.png) 
[screenshot9](screenshots'/3day9.png) 
[screenshot10](screenshots'/3day10.png) 
[screenshot11](screenshots'/3day11.png) 

