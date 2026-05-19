## Overview
Hardened the FTP service on Metasploitable after 
exploiting it in entry 03.

## Changes Made
- Disabled anonymous FTP login in vsftpd.conf
- Upgraded vsftpd to patched version
- Blocked port 21 with iptables as additional layer

## Commands
```bash
sudo nano /etc/vsftpd.conf
# Set: anonymous_enable=NO
sudo /etc/init.d/vsftpd restart
sudo iptables -A INPUT -p tcp --dport 21 -j DROP
```

## Verified
Re-ran Metasploit exploit — no session created.

## What I Learned
Defense in depth: patch the vulnerability AND restrict 
access at the firewall level. One layer is never enough.
