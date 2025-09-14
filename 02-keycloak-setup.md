# 02 – Keycloak Setup

This document explains the installation and configuration of **Keycloak** on the Rocky Linux VM.

---

## Installation Steps

### 1. Install Java 17
```bash
sudo dnf install -y java-17-openjdk
java -version
2. Download and Extract Keycloak
bash
Copy code
cd /opt
sudo curl -L -O https://github.com/keycloak/keycloak/releases/download/24.0.4/keycloak-24.0.4.zip
sudo dnf install -y unzip
sudo unzip keycloak-24.0.4.zip
sudo mv keycloak-24.0.4 keycloak
3. Create Keycloak User
bash
Copy code
sudo useradd -r -s /sbin/nologin keycloak
sudo chown -R keycloak:keycloak /opt/keycloak
4. Start Keycloak in Development Mode
bash
Copy code
sudo -u keycloak /opt/keycloak/bin/kc.sh start-dev --http-port=8080 --hostname=0.0.0.0
⚠️ Dev mode is used for testing only. Production requires HTTPS certificates.

5. Open Firewall Port
bash
Copy code
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
6. Create Admin User
When prompted:

makefile
Copy code
Username: admin
Password: ********
7. Access Admin Console
In browser:

cpp
Copy code
http://<VM_IP>:8080
Login with the admin credentials.

Screenshots
Keycloak dev mode running:

Login page:

Admin Console dashboard:


