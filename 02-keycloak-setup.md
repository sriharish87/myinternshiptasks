# Keycloak Setup (Day 3)

This script documents the steps to install and configure Keycloak on Rocky Linux.

# Step 1 – Install Java 17
sudo dnf install -y java-17-openjdk
java -version

# Step 2 – Download and Extract Keycloak
cd /opt
sudo curl -L -O https://github.com/keycloak/keycloak/releases/download/24.0.4/keycloak-24.0.4.zip
sudo dnf install -y unzip
sudo unzip keycloak-24.0.4.zip
sudo mv keycloak-24.0.4 keycloak

# Step 3 – Create Keycloak User
sudo useradd -r -s /sbin/nologin keycloak
sudo chown -R keycloak:keycloak /opt/keycloak

# Step 4 – Start Keycloak (Dev Mode)
sudo -u keycloak /opt/keycloak/bin/kc.sh start-dev --http-port=8080 --hostname=0.0.0.0
# ⚠️ Dev mode is for testing only. Production requires HTTPS and certificates.

# Step 5 – Open Firewall Port
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload

# Step 6 – Create Admin User
# When prompted:
# Username: admin
# Password: ********

# Step 7 – Access Admin Console
# Open your browser at:
# http://<VM_IP>:8080
# Login with the admin credentials.

# Screenshots (to replace later with real ones)
# ![Keycloak start](screenshots/keycloak-start.png)
# ![Keycloak login](screenshots/keycloak-login.png)
# ![Keycloak dashboard](screenshots/keycloak-dashboard.png)


