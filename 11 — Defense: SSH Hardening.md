## Overview
Hardened SSH after brute force attack in entry 08.

## Changes Made
- Disabled root login over SSH
- Set MaxAuthTries to 3
- Disabled password authentication
- Installed and configured fail2ban

## Commands
```bash
sudo nano /etc/ssh/sshd_config
# PermitRootLogin no
# MaxAuthTries 3
# PasswordAuthentication no
sudo apt install fail2ban -y
sudo /etc/init.d/fail2ban start
sudo /etc/init.d/ssh restart
```

## Verified
Re-ran Medusa brute force — IP banned after 3 attempts.

## What I Learned
SSH hardening is one of the most impactful things you 
can do on any Linux server. fail2ban alone stops most 
automated attacks dead.
