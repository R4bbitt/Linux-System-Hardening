
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

Now that we know a bit more about our operating system it's always best practice to make a backup before making any big changes. I started with running ls -la in /, /home, and because there are only a few users I briefly inspected each user's folder to get a quick overview of what’s in each folder. By running the following command I created a tarball, or backup, excluding a list of unnecessary folders.

- sudo tar -cvpzf /baker_street_backup.tar.gz --exclude=/baker_street_backup.tar.gz --exclude=/proc --exclude=/tmp --exclude=/mnt --exclude=/sys --exclude=/dev --exclude=/run
- use ls to double check the backup was created

<img width="787" height="165" alt="Image" src="https://github.com/user-attachments/assets/99fe5305-78eb-498a-8b14-dad871c15d19" />

# Audit Users and Groups

Next I removed users that are no longer employed along with their corresponding files.
- Sudo deluser - -remove-all-files lestrade <br>
I ran into issues as the command removed the users id and the –remove-home wouldn’t work, so I used:
- Sudo rm -r lestrade <br>
This removed lestrade’s home folder and subdirectories recursively.

<img width="687" height="77" alt="Image" src="https://github.com/user-attachments/assets/8cf139f5-aa70-4367-bbb4-db5d4188360a" />

I then used the following bash on all other users as it was far quicker 
- Deluser - -remove-home [user]

<img width="678" height="60" alt="Image" src="https://github.com/user-attachments/assets/977c82f0-6959-4857-9336-add15c519448" />

Once lestrade and irene were removed I moved on to locking and unlocking users, first checking each account's status.
- Passwd -S [user]

<img width="692" height="104" alt="Image" src="https://github.com/user-attachments/assets/68a2edbd-4e58-4879-ada4-aad8b4d083cc" />

To lock moriarty I used: 
- Usermod -L moriarty 

<img width="693" height="82" alt="Image" src="https://github.com/user-attachments/assets/9dbcf0e7-02eb-4aab-8818-bc7ed0103fc0" />

While unlocking Toby and Adler I discovered their accounts did not have assigned passwords. As it would be a security risk to have unrestricted access into these accounts I assigned them a new password using: 
- Passwd [user]

<img width="667" height="181" alt="Image" src="https://github.com/user-attachments/assets/c2946d28-27b6-460e-abae-2a401341c1e8" />

I then unlocked the locked accounts and added users to their respective groups. 
- usermod -U [user] to unlock users
- Groupadd research to add the research group 

<img width="732" height="127" alt="Image" src="https://github.com/user-attachments/assets/1a7e01f7-08de-4413-9bcc-3149e935b1c7" />

# Implement Password Policy

I added the new prerequisites to user passwords, requiring a minimum length of 8 characters, at least 1 unique character, at least 1 uppercase letter, and only allowing 2 login attempts. I used nano to edit the passwords file.
- nano /etc/pam.d/common-password
  
<img width="694" height="224" alt="Image" src="https://github.com/user-attachments/assets/a33e6166-dbb5-4c60-bdf2-df8f8b7658c4" />

# Update Sudo Permissions

I removed illegitimate users and edited watson’s and mycroft’s abilities, giving them access to run sudo only when running the logcleanup script and the research script for only those in the research group.
- Sudo visudo

<img width="694" height="209" alt="Image" src="https://github.com/user-attachments/assets/3ed50b34-04f1-4192-8359-facb8a3dd4d1" />

Additionally, users now have to enter their password for sudo commands adding another layer of security for running sudo.

<img width="674" height="189" alt="Image" src="https://github.com/user-attachments/assets/98febafc-e8d5-4639-8835-c207a2d2cfaa" />

# Update Permissions on Files and Directories

Next I need to update the ownership of two scripts, assigning them to the engineering group.
- Chgrp engineering [.sh file]

<img width="690" height="71" alt="Image" src="https://github.com/user-attachments/assets/50ac3100-43d3-43d8-9571-3b24b91d78bd" />


Based on information of our members, some have improper read write priveleges on files and scripts. We want to make sure only members of a certain group have access to their respective files. In hindsight this was a process I could have streamlined for more effciency. 
- Chmod 070 [.sh file] to update rwx permissions
- Chmod 640 [file] to remove world privileges but keeping the original read and write privileges on all other files.

<img width="682" height="312" alt="Image" src="https://github.com/user-attachments/assets/b354da48-3789-46ea-90e7-15ebbc42175c" />

Using the same set of commands I continued to update permissions for moriarty.

<img width="674" height="266" alt="Image" src="https://github.com/user-attachments/assets/dd22beb0-991f-4b01-adc7-4191e43c8a4d" />

I continued this process for all other users.

# Audit SSH

Now I need to inspect the SSH settings enabling Port 22 so we can SSH into it if we need to. All I need to do for this is to uncomment that line in the config file.
- Nano /etc/ssh/sshd_config 

<img width="685" height="143" alt="Image" src="https://github.com/user-attachments/assets/5e662879-45ae-49c2-b2b5-808d95397571" />

Disabled root login

<img width="675" height="87" alt="Image" src="https://github.com/user-attachments/assets/88cae5ed-c5c7-4c2b-8189-eec433f56068" />

Require a password for ssh connection

<img width="679" height="62" alt="Image" src="https://github.com/user-attachments/assets/85cdc826-6c80-4b68-9d53-e8a114a22ff6" />

Added protocol 2 and then restart the shell server to apply the changes.
- Service ssh restart

<img width="669" height="136" alt="Image" src="https://github.com/user-attachments/assets/a9e2f294-6aff-4c35-ac25-04c5eebe23bd" />

<img width="671" height="63" alt="Image" src="https://github.com/user-attachments/assets/b9791e42-1d13-4519-a974-9be6c413ca20" />

# Review System Packages

Now I need to view the package lists and check to see what needs to be updated.
- apt update
- Touch package_list.txt to create a file that will have a list of all of our packages.
- Apt list –installed > package_list.txt to print out a list of installed programs to our new file.
- Chmod 640 [file] to remove world privileges from that new file.

Our system appears to be using telnet and rsh-client. These programs are proven to be fairly insecure so for the integrity and security of this company we will remove them.

-apt remove telnet




