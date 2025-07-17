# Ansible - NGINX on Ubuntu

This repository contains playbooks for basic UFW configuration, updates and NGINX configuration.

## Repository structure

### inventory/

This Directory contains connection details for all targeted hosts.

#### hosts.yml

This YML file currently containes configuration for my local setup build with Virtual Box.
___

### playbooks/

This directory contains all playbooks needed.

#### apt_upgrade.yml

This playbook is used to update `apt` cache and upgrade packages.

`sudo apt update && sudo apt upgrade`

#### ufw_setup.yml

This playbook is used to install UFW. Set rules to allow SSH, HTTP and HTTPS and deny anything else + enable UFW.

#### ubuntu_init_setup.yml

This playbook is used to do initial configuration of Ubuntu Server and Install essential packages.

**THIS PLAYBOOK USES BOTH apt_upgrade.yml and ufw_setup.yml**

Installed packages:
* `net-tools` - package for networking
* `htop` - package for monitor server load
* `unattended-upgrades` - package for automated updates.
* `fail2ban` - package for defense against DDoS attacks.

Next it configures several security options:
* Disabled direct root login.
* Login can only be done with SSH keys.
* SSH login has maximum of 5 attempts and then user is banned for 1 hour
* Configured automatic security updates

`ansible-playbook playbooks/ubuntu_init_setup.yml -i inventory/hosts.yml --ask-vault-pass --ask-become-pass`

#### nginx_setup.yml

This playbook is used to upload SSH key encrypted with `ansible-vault` and use it to clone static site from `git` repository and place it inside `/opt/static-page/` with ownership being set to `webapp:webteam`.  
Another part of this playbook handles NGINX installation and change of default config for our own.

`ansible-playbook playbooks/nginx_setup.yml -i inventory/hosts.yml --ask-vault-pass --ask-become-pass`
___

### vault/

This directory contains sensitive files encrypted by ansible-vault.

#### id_rsa

SSH key encrypted with `ansible-vault` and used for `git` authentication.
___

### .gitignore

Git ignore for VS Code.

### README.md

The file you are currently reading.
___

## How to use it

ansible-playbook playbooks/ubuntu_init_setup.yml -i inventory/hosts.yml --ask-vault-pass --ask-become-pass

`ansible-playbook path_to/playbook.yml -i path_to/inventory.yml --ask-vault-pass --ask-become-pass`