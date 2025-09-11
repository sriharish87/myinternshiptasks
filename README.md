# Internship Project – Keycloak SSO Integration

This repository documents my internship tasks on setting up a secure Rocky Linux server, installing Keycloak, and integrating it with multiple applications (Drupal, Django, PHP).  

I used **VirtualBox** with Rocky Linux 10 for this project (instead of DigitalOcean droplet). The focus is on **step-by-step reproducible documentation** and screenshots.

---

## 📌 Repository Structure
- [01-server-setup.md](01-server-setup.md) – Rocky Linux server setup  
- [02-keycloak-setup.md](02-keycloak-setup.md) – Keycloak installation & configuration  
- [03-drupal-integration.md](03-drupal-integration.md) – Drupal 11 + Keycloak SSO  
- [04-django-integration.md](04-django-integration.md) – Django app + Keycloak SSO  
- [05-php-integration.md](05-php-integration.md) – PHP app + Keycloak login  
- [progress.md](progress.md) – Daily progress log  
- [screenshots/](screenshots/) – Screenshots for each step  
- `.github/workflows/daily-commit.yml` – Automated daily GitHub commit  
- `.gitignore` – Ignore unnecessary files  

---

## 🚀 Objectives
- Provision a secure server environment (VirtualBox Rocky Linux 10 VM).  
- Install and configure Keycloak for identity management.  
- Deploy and integrate applications with SSO:
  - Drupal 11  
  - Django application  
  - PHP web app  
- Document all steps in Markdown files for easy reference.  
- Automate daily commits to GitHub for activity tracking.  

---

## 🛠️ Tools & Technologies
- VirtualBox (VM hosting)  
- Rocky Linux 10 (Server OS)  
- Apache HTTP Server  
- MariaDB (Database)  
- PHP 8.2  
- Python 3.11  
- Keycloak (Identity & Access Management)  
- GitHub Actions (Automated daily commits)  

---

## 📖 Documentation Flow
1. Server Setup (Rocky Linux on VirtualBox)  
2. Keycloak Installation  
3. Drupal Integration  
4. Django Integration  
5. PHP Integration  

---

## ✅ Daily Commit Automation
This repository includes a GitHub Actions workflow (`.github/workflows/daily-commit.yml`) that creates an automated empty commit once per day. This ensures activity tracking for internship evaluation.  

---

## 👨‍💻 Author
**Sri Harish**  
Internship Project – Keycloak SSO Integration

