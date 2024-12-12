---
title: How to use Docker commands without sudo
date: 2024-11-09 12:00:00 -0700
categories: [docker]
tags: [docker, linux, debian, ubuntu]
---

Log into the the Linux user you want to use and run this command:
```
sudo usermod -aG docker $(whoami)
```
Log out and back in.
Now you can run commands like `docker ps` without using `sudo`.

## Extra - Breakdown how how the command works.
> sudo:

This part executes the following command with root privileges.
Since modifying user groups requires administrative access, sudo is necessary. If you are running this command from root, you will not need sudo.

>usermod:

This is the command used to modify user account properties, such as group memberships.

>-a:

This option tells usermod to append the user to the specified group.
If the user is already a member of other groups, this option ensures they remain in those groups while also being added to the docker group.

>-G docker:

This specifies the group to which the user should be added: the docker group.
This group has the necessary permissions to interact with Docker commands and services.

>$(whoami):

This is command substitution.
whoami is a command that returns the current username.
The `$()` syntax executes the whoami command and substitutes its output (the username) into the usermod command. You may also explicitly specify the user by replacing `$(whoami)` with the user_name.