# Day 6 – PHP App Integration with Keycloak SSO

## 1) Prepare PHP App Directory

```bash
sudo mkdir -p /var/www/php_app
sudo chown -R apache:apache /var/www/php_app
sudo chmod -R 775 /var/www/php_app
Why: Ensures Apache can read/write files for the PHP app.
```
2) Install PHP OpenID Connect Library
```bash
Copy code
cd /var/www/php_app
composer require jumbojett/openid-connect-php
```
Why: This library handles Keycloak login and OIDC authentication.

3) Create Example PHP Files
login.php

php
```Copy code
<?php
require_once __DIR__ . '/vendor/autoload.php';

$oidc = new \Jumbojett\OpenIDConnectClient(
    'https://<keycloak-server>/realms/<realm-name>',
    '<client-id>'
);

$oidc->authenticate();
$userinfo = $oidc->requestUserInfo();

echo "Hello, " . $userinfo->preferred_username;
profile.php
````
php
```
Copy code
<?php
session_start();
echo "Logged in as: " . $_SESSION['username'];
```
Why: Basic login and profile display example using Keycloak SSO.

4) Configure Keycloak Client
Go to Keycloak admin console → Clients → Create php-app

Set Redirect URI: http://<php-domain-or-ip>/callback.php

Configure Access Type: confidential or public depending on your setup

5) Apache Configuration
Add a virtual host for PHP app if needed:
```
apache
Copy code
<VirtualHost *:80>
    ServerName php-app.local
    DocumentRoot /var/www/php_app
    <Directory /var/www/php_app>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
Restart Apache:
```bash
Copy code
sudo systemctl restart httpd
```
6) Test Login
Open browser: http://<php-domain-or-ip>/login.php

Expected: Redirect to Keycloak login → redirect back to profile.php

⚠ Note
Despite following all steps, I was unable to connect with Keycloak. I tried different approaches, but the PHP app could not successfully authenticate. This issue may require further debugging or Keycloak client configuration review
screenshots  
[screenshot1](screenshots'/6day1.png)  
[screenshot2](screenshots'/6day2.png)  
