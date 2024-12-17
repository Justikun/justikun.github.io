---
title: Checklist Setup For A New Debian Server
date: 2024-11-08 12:00:00 -0700
categories: [Linux]
tags: [linux, debian, ubuntu, ufw, serversetup, ssh, firewall]
---

This checklist outlines the initial steps I take when setting up a fresh Debian or Ubuntu server. I typically use Proxmox for creating virtual machines, but these steps are applicable regardless of your chosen method.

### 1. Create a new user

>You may not need to create a new user in this step if you already did this when setting up your Linux machine.
{: .prompt-info }

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
>If you decide to change your port number, please document it so you don't forget about it.
{: .prompt-danger }

The default SSH port (22) is well-known, making it a prime target for brute-force attacks. Let's enhance security by changing the SSH port to a less common one.

```bash
sudo nano /etc/ssh/sshd_config
```
Locate the `#Port 22` line and uncomment it. Then, change the port number to your an uncommon port (e.g., Port 51234). I like to choose something between 50000-60000.
```
# Port 22
Port 51234
```

Save and exit the file by pressing `CTRL + X`, then `Y`, and finally Enter.

### 4. Restart the ssh server 
To apply the changes to the SSH configuration, restart the SSHD service:
```bash
sudo systemctl restart sshd
```

### 5. Enable Firewall
A firewall is essential for securing your server. We'll start with a default deny rule for incoming traffic and then explicitly allow only the necessary connections.
```bash
sudo ufw default deny incoming
sudo ufw allow from 192.168.1.0/24 to any port 51234
sudo ufw default allow outgoing

sudo ufw enable
```
This configuration blocks all incoming network traffic by default, except for SSH connections originating from your local network. Replace '192.168.1.0/24' with the actual subnet of your local network. Then we allow all outgoing traffic from the server. And finally we enable the ufw firewall.

>Note: These firewall rules are a basic starting point. You'll likely need to adjust them based on your specific project requirements and security needs. You will also need to ensure your router's firewall is up and configured.
{: .prompt-warning }

I hope this revised checklist helps you quickly set up the basics of your new Debian server.