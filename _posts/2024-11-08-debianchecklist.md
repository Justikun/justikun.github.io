---
title: Checklist Setup For A New Debian Server
date: 2024-11-08 12:00:00 -0700
categories: [linux]
tags: [linux, debian, ubuntu]
---

This checklist outlines the initial steps I take when setting up a fresh Debian or Ubuntu server. I typically use Proxmox for creating virtual machines, but these steps are applicable regardless of your chosen method.

### 1. Create a new user
> [!info] 
You may not need to create a new user in this step if you already did this when setting up your Linux machine.

We'll start by creating a new user, `justin`.

The second line adds `justin`, to the `sudo` group, granting the user the ability to still execute commands with root privelages using `sudo`. This enhances security and promotes best practices.
```bash
adduser justin
usermod -aG sudo justin
 ```
### 2. Update the OS
We want the latest OS update for the latest security patches.
```bash
sudo apt update ; sudo apt upgrade
```
### 3. Change the SSH port number
>[info] If you decide to change your port number, please document it.

Changing the port number can generally help with automated scanners. It's not perfect, but it helps a little bit.

You can edit the port number with the following command:
```bash
sudo nano /etc/ssh/sshd_config
```
Inside the editor use the arrow keys to move around. Uncomment, and change the port number from 22 to an uncommon port number like 51234. I like to choose something between 50000-60000. But any port will do. 

Save and exit the file by pressing `Ctrl+X`, then `Y`, and finally Enter.

### 4. Restart the ssh server 
To apply the changes to the SSH configuration, restart the SSHD service:
```bash
sudo systemctl restart sshd
```

### 5. Enable Firewall
A firewall is essential for securing your server. We'll start with a default deny rule for incoming traffic and then explicitly allow only the necessary connections.
```
sudo ufw default deny incoming
sudo ufw allow from 192.168.1.0/24 to any port 51234
sudo ufw default allow outgoing

sudo ufw enable
```
This configuration blocks all incoming network traffic by default, except for SSH connections originating from your local network. Replace '192.168.1.0/24' with the actual subnet of your local network. All outgoing traffic from the server is permitted.

>[!note] Note: These firewall rules are a basic starting point. You'll likely need to adjust them based on your specific project requirements and security needs.`

I hope this revised checklist helps you quickly set up the basics of your new Debian server.