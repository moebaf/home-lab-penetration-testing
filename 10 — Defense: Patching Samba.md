## Overview
Mitigated CVE-2007-2447 by disabling the vulnerable 
username map script feature in Samba.

## Changes Made
- Cleared username map script value in smb.conf
- Restarted Samba service
- Blocked port 445 externally with iptables

## Commands
```bash
sudo nano /etc/samba/smb.conf
# Under [global]: username map script =
sudo /etc/init.d/samba restart
```

## Verified
Re-ran Metasploit exploit — connection refused, no shell.

## What I Learned
A single misconfigured line in a config file gave root 
access. Always audit service configurations after install.
