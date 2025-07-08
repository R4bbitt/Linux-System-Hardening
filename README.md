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





