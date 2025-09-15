04-django-integration.md
# Day 5 – Django App + Keycloak SSO Integration

## 1. Create MariaDB database for Django
```bash
sudo mysql -u root -p
```

Inside MySQL shell:
```
CREATE DATABASE django_db;
CREATE USER 'django_user'@'localhost' IDENTIFIED BY 'StrongPass123!';
GRANT ALL PRIVILEGES ON django_db.* TO 'django_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
2. Install Django + Gunicorn + OIDC library
```
sudo mkdir -p /var/www/django_project
sudo chown $USER:$USER /var/www/django_project
cd /var/www/django_project

python3 -m venv venv
source venv/bin/activate

pip install django gunicorn mozilla-django-oidc mysqlclient

```
Create project:
```
django-admin startproject mysite
cd mysite


Edit mysite/settings.py:

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_db',
        'USER': 'django_user',
        'PASSWORD': 'StrongPass123!',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}

INSTALLED_APPS += ['mozilla_django_oidc']

AUTHENTICATION_BACKENDS = (
    'mozilla_django_oidc.auth.OIDCAuthenticationBackend',
    'django.contrib.auth.backends.ModelBackend',
)

LOGIN_URL = '/oidc/authenticate/'
LOGIN_REDIRECT_URL = '/'
LOGOUT_REDIRECT_URL = '/'

OIDC_RP_CLIENT_ID = "django"
OIDC_RP_CLIENT_SECRET = "<paste_your_client_secret_here>"
OIDC_OP_AUTHORIZATION_ENDPOINT = "http://<keycloak_ip>:8080/realms/master/protocol/openid-connect/auth"
OIDC_OP_TOKEN_ENDPOINT = "http://<keycloak_ip>:8080/realms/master/protocol/openid-connect/token"
OIDC_OP_USER_ENDPOINT = "http://<keycloak_ip>:8080/realms/master/protocol/openid-connect/userinfo"
OIDC_OP_JWKS_ENDPOINT = "http://<keycloak_ip>:8080/realms/master/protocol/openid-connect/certs"
```

Run migrations:
```
python manage.py migrate
python manage.py createsuperuser
```
3. Create Gunicorn systemd service

Create file:
```
sudo nano /etc/systemd/system/django.service
```

Paste:
```
[Unit]
Description=Gunicorn instance to serve Django app
After=network.target

[Service]
User=apache
Group=apache
WorkingDirectory=/var/www/django_project/mysite
ExecStart=/var/www/django_project/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 mysite.wsgi:application

[Install]
WantedBy=multi-user.target

```
Enable service:
```
sudo systemctl daemon-reload
sudo systemctl enable django
sudo systemctl start django
```
4. Configure Apache Reverse Proxy

Create file:
```
sudo nano /etc/httpd/conf.d/django.conf
```

Paste:
```
<VirtualHost *:80>
    ServerName your_django_domain_or_ip

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:8000/
    ProxyPassReverse / http://127.0.0.1:8000/

    ErrorLog /var/log/httpd/django_error.log
    CustomLog /var/log/httpd/django_access.log combined
</VirtualHost>
```

Restart Apache:
```
sudo systemctl restart httpd
```
5. Create Keycloak client for Django

Open Keycloak Admin Console

Clients → Create

Client ID: django

Valid Redirect URIs:
```
http://<your_django_domain_or_ip>/oidc/callback/
```

Copy the Client Secret and paste into settings.py.

6. Test Login

Visit Django app at http://<your_server_ip>/

Click Login → you should be redirected to Keycloak

After login, you are redirected back to Django as authenticated user

7. Screenshot requirements  
   [screenshot1](screenshots'/5day1.png)  
   [screenshot2](screenshots'/5day2.png)  
   [screenshot3](screenshots'/5day3.png)
   [screenshot4](screenshots'/5day4.png)  
   [screenshot5](screenshots'/5day5.png)  
   [screenshot6](screenshots'/5day6.png)    
