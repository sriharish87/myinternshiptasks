
# Day 3 – Drupal 11 + Keycloak SSO Integration

## 1. Create MariaDB database for Drupal

```bash
sudo mysql -u root -p
```

Inside MySQL shell:
```
CREATE DATABASE drupal_db;
CREATE USER 'drupal_user'@'localhost' IDENTIFIED BY 'StrongPass123!';
GRANT ALL PRIVILEGES ON drupal_db.* TO 'drupal_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
2. Install Drupal
cd /var/www
composer create-project drupal/recommended-project drupal
sudo chown -R apache:apache drupal


⚠️ If composer warns “Do not run composer as root”, type y to continue.

3. Configure Apache Virtual Host

Create file:

sudo nano /etc/httpd/conf.d/drupal.conf


Paste:

<VirtualHost *:80>
    ServerName your_django_domain_or_ip
    DocumentRoot /var/www/drupal/web

    <Directory /var/www/drupal/web>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/httpd/drupal_error.log
    CustomLog /var/log/httpd/drupal_access.log combined
</VirtualHost>


Enable and restart Apache:

sudo systemctl restart httpd

4. Run Drupal Installer

Open browser:

http://<your_server_ip>/


Select Standard profile

Enter DB info:

Database: drupal_db

User: drupal_user

Password: StrongPass123!

5. Install Keycloak module

Inside project:

cd /var/www/drupal
composer require drupal/keycloak


Enable module:

drush en keycloak -y

6. Create Keycloak client for Drupal

Go to Keycloak Admin Console

Clients → Create:

Client ID: drupal

Valid Redirect URIs:

http://<your_server_ip>/keycloak/oauth2/callback


Copy the Client Secret

7. Configure Drupal Keycloak

In Drupal admin → Configuration → Keycloak settings:

Keycloak Base URL: http://<keycloak_ip>:8080/

Realm: master

Client ID: drupal

Client Secret: paste from Keycloak

Redirect URI:

http://<your_server_ip>/keycloak/oauth2/callback


Save config.

8. Test Login

Go to Drupal login page

Click Login with Keycloak

Enter Keycloak credentials

You should be redirected back and logged into Drupal
