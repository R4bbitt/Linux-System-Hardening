# Linux-System-Hardening
This project served as a learning experience where I implemented my knowledge of linux in hardening a system and its users and groups. I implemented and enforced a password policy and applied the principle of least privelege, minimizing potential risks from insider threats and/or bad actors.

## Steps I took:

* [Gather OS Information](#gather-os-information)
* [Create a Backup](#create-a-backup)
* [Audit Users and Groups](#audit-users-and-groups)
* [Implement Password Policy](#implement-password-policy)
* [Update Sudo Permissions](#update-sudo-permissions)
* [Update Permissions on Files and Directories](#update-permissions-on-files-and-directories)
* [Audit SSH](audit-ssh)
* [Review System Packages](#review-system-packages)
* [Disbale Unnecessary Services](#disable-unnecessary-services)
* [Configure Logging](#configure-logging)
* [Create and Schedule Hardening Scripts](#create-and-schedule-hardening-scripts)

_________________

# Gather OS Information

Using a few simple bash commands we can get a lot of useful information about the system we are working on. In this scenario our customer/company is the Baker Street Corporation.

#### Bash Commands Used
- hostname: Baker_Street_Linux_Server
- hostnamectl: 5.15.0-1067-aws
- top: Total=16182800 Used=1378584 Free=10903876
- uptime: 01:05:39 up 34 min, 0 users, load avg 0.05, 0.14, 0.20

# Create a Backup

Now that we know a bit more about our operating system it's always best practice to make a backup before making any big changes. I started with running ls -la in /, /home, and because there are are only a few users I briefly inspected each user's folder to get a quick overview of what’s in each folder. By running the following command I created a tarball, or backup, excluding a list of unnecessary folders.

- sudo tar -cvpzf /baker_street_backup.tar.gz --exclude=/baker_street_backup.tar.gz --exclude=/proc --exclude=/tmp --exclude=/mnt --exclude=/sys --exclude=/dev --exclude=/run
- use ls to double check the backup was created
[
](https://github.com/R4bbitt/Linux-System-Hardening/edit/main/README.md#:~:text=README.md-,Screen,-Shot%202025%2D02)
# Audit Users and Groups





