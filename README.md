# Ansible playbook to install Graylog on Ubuntu 20.04 server.


## Description

The following Ansible playbook, will install Elasticsearch engine (for logs storage), MongoDB (to store Graylog's settings), Graylog server (for log's management, monitoring, analysis and alerting) and Nginx web server (as proxy server to access Graylog web interface on server's port 80) on Ubuntu 20.04 server.<br />
At the last step, three firewall rules (custom SSH port, http and https ports) will be setup to further protect Graylog server.<br />
A further security step, could be to password protect web root's folder (see Apache's htpasswd module).

## Components

* Elasticsearch (ver. 6)
* MongoDB
* Graylog (ver. 3.3)
* Nginx (as proxy server)


## Tested on

Ubuntu 20.04 server (should work on Debian 10 as well).


## Prerequisites

a) A remote server with Ubuntu 20.04 and bootstrap setup already performed (See my bootstrap playbook at: https://github.com/jobetinfosec/ansible-ubuntu2004-bootstrap)<br />
b) An SSH key pair on your laptop or Ansible remote server, with the public key already installed in the remote server<br />
d) Ansible already installed on your laptop or Ansible remote server


## Instructions

a) On your laptop or Ansible remote server, create a working directory


b) In your working directory open a console window


c) Clone github repository

```
git clone https://github.com/jobetinfosec/ansible-ubuntu2004-graylog.git
```


d) Switch to playbook directory

```
cd ansible-ubuntu2004-graylog
```


e) Open the hosts file

```
nano hosts
```

f) Replace <TEMPORARY_ITEMS> with your own data:

| Item | Instructions | Further info |
| :---: | :---: | :---: |
| `<SERVER_IP>` | replace it with your remote server's IP address |  |
| `<SUDO_USER_NAME>` | replace it with sudo user's name |  |
| `<SUDO_USER_PASSWORD>` | replace it with sudo user's password |  |
| `<SSH_PRIVATE_KEY_NAME>` | replace it with your SSH private key name | For example: ~/.ssh/key_name |


then save and close the file


g) Open the roles/graylog/defaults/main.yml file

```
nano roles/graylog/defaults/main.yml
```

h) Replace <TEMPORARY_ITEMS> with your own data:

| Item | Instructions | Further info |
| :---: | :---: | :---: |
| `<GRAYLOG_SECRET_KEY>` | replace it with the secret key created on your laptop | With the command "pwgen -N 1 -s 96" create a strong password on your laptop (if pwgen is not installed, install it with "apt-get install pwgen"). |
| `<GRAYLOG_ADMIN_PASSWORD_HASH>` | replace it with the hash of Graylog admin user's password | First create on your laptop, a strong password. Then create a password hash using the command "echo -n <PASSWORD> \| sha256sum" replacing <PASSWORD> with your password |
| `WEB_DOMAIN` | replace it with the web domain name used for Graylog's server | For example: graylog.domain.com |
| `ADMIN_IP` | replace it with the outbound IP address of system administrator | |
| `CUSTOM_SSH_PORT` | replace it with your custom SSH port number | |

then save and close the file


i) Check if you are able to ping remote server

```
ansible -m ping all
```

You should receive a SUCCESS message


j) Check if any error shows up

```
ansible-playbook graylog.yml --check
```


k) Launch installation

```
ansible-playbook graylog.yml
```


## Licence

MIT licence

## Author Information

Created by Roberto Jobet (sysadmin@wpsecurity.press).

Don't hesitate to open an Issue if you find any bug or have suggestions.
