---
title: "Setting OpenSSH server on WSL"
categories:
  - Linux
tags:
  - setup
  - config
---

Setup ssh server on windows linux sub-sytem.

1. Prerequisite:

- The lastest windows 10 installed.

- Windows System Linux is enabled and installed.

2. Remove (optinal) and reinstall openssh server

```bash
sudo apt install openssh-server

```

3. Edit the sshd_config

- The config file at */etc/ssh/sshd_config*

- Should be edited changed the following:

```
Port 2222
ListenAddress 0.0.0.0
PasswordAuthentication yes
AllowUsers <your_linux_user_name>
PermitRootLogin no
```
3.1 Generate ssh key

```bash
ssh-keygen -A
ssh-keygen -t rsa -C "taihv@localhost"
```

4. Restart ssh service

```bash
sudo service ssh --full-restart
```

5. Setup windows firewall, "Inbound Rules" allows port 2222

(TODO)

6. Testing

- Open ssh client (PuTTY, Tera Term, ...), I am using git bash with ssh client installed

- Just run `ssh taihv@T480 -p 2222`

- and then confirm connection (if any), input password

7. Enjoy

