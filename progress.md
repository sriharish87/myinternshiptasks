✅ Day 1 – Server Setup

Installed Rocky Linux 10 on VirtualBox.

Configured VM (2GB RAM, 20GB disk, 2 cores).

Performed system updates and installed core packages (vim, wget, curl, git, unzip, tar, net-tools).

Verified terminal access.

📸 Screenshots:

[updatepage](screenshots'/update.png)
[newuser](screenshots'/newuser.png)
[firewallsetup](screenshots'/firewallsetup.png)
[corepackage](screenshots'/installedcorepackages.png)
[corepackages2](screenshots'/installedcorepackages2.png)

📌 Day 2 – Keycloak Setup

Install Java JDK (required for Keycloak).

Download and configure Keycloak latest version.

Setup systemd service for Keycloak.

Verify Keycloak is running at http://localhost:8080.

📸 Screenshots:

(to be added)

📌 Day 3 – Drupal Integration

Install Apache and MariaDB.

Setup Drupal 11.

Configure Keycloak SSO Module for Drupal.

📸 Screenshots:

(to be added)

📌 Day 4 – Django Integration

Setup Python virtual environment.

Install Django + dependencies.

Configure app behind Apache (reverse proxy).

Add Keycloak authentication integration.

📸 Screenshots:

(to be added)

📌 Day 5 – PHP Integration

Install PHP 8.2 and Apache.

Deploy sample PHP web app.

Configure Keycloak login for PHP app.

📸 Screenshots:

(to be added)

📌 Final Notes

All steps documented in Markdown files (01-server-setup.md, 02-keycloak-setup.md, etc).

Daily commits are automated via GitHub Actions.

Repository includes screenshots/ folder with evidence of progress.
