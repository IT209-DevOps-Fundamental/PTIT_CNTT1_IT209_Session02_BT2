# User Management and SSH Configuration Report

## Overview
This document outlines the administrative Linux commands executed to provision a new system user, configure secure SSH key-based authentication, assign appropriate directory permissions, and verify administrative (sudo) privileges.

## Execution Steps

### 1. User Account Creation
The following commands were used to create a new standard user and grant it administrative privileges by adding it to the `sudo` group.

```bash
# Create a new user account
adduser newuser

# Append the new user to the sudo group
usermod -aG sudo newuser
```

### 2. SSH Key Migration
To enable secure, passwordless authentication for the new user, the SSH configuration directory was created, and the public keys were copied from the root account.

```bash
# Create the .ssh directory within the new user's home directory
mkdir -p /home/newuser/.ssh

# Copy the authorized_keys file containing the public SSH keys
cp /root/.ssh/authorized_keys /home/newuser/.ssh/
```

### 3. Permission Enforcement
SSH daemons require strict permission settings for key files. The following commands ensure the correct ownership and access rights are applied to prevent security vulnerabilities.

```bash
# Recursively change the ownership of the .ssh directory to the new user
chown -R newuser:newuser /home/newuser/.ssh

# Restrict access to the .ssh directory (owner can read, write, and execute)
chmod 700 /home/newuser/.ssh

# Restrict access to the authorized_keys file (owner can read and write)
chmod 600 /home/newuser/.ssh/authorized_keys
```

### 4. Privilege Verification
To validate the configuration, we switched to the new user account and executed a command requiring root privileges.

**Terminal Execution Log:**

```text
root@server:~# su - newuser
newuser@server:~$ sudo whoami
[sudo] password for newuser: 
root
newuser@server:~$ 
```

***
*End of report*
