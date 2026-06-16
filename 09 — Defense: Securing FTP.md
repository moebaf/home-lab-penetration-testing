# 03 — Defense: Securing the FTP Port

## Environment
- Target: Metasploitable 2 192.168.56.101
- Date: 16 May 2026

## Overview
Removed the vulnerable vsftpd 2.3.4 service and replaced it 
with a patched version to eliminate the backdoor exploited 
in entry 02.

## Tool
nmap (to verify the fix)

## Commands

```bash
sudo apt-get remove vsftpd
sudo apt-get install vsftpd=2.3.5
nmap -sV 192.168.56.101
```

## Output

```text
PORT     STATE SERVICE     VERSION
21/tcp   open  tcpwrapped
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
...
(vsftpd 2.3.4 no longer appears in scan results)
```

## Findings
After removing and upgrading vsftpd, the vulnerable version 
2.3.4 no longer appears in the Nmap scan. The port still shows 
as tcpwrapped but the backdoor service is gone, meaning the 
exploit from entry 02 would no longer work.

## What I Learned
Keeping software updated to the latest version removes known 
vulnerabilities. Removing software you no longer need reduces 
the attack surface further. Always verify a fix by re-scanning 
after making changes — never assume a patch worked without 
checking.
