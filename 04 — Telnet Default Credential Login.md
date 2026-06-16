# 05 — Telnet Default Credential Login

## Environment
- Attacker: Kali Linux 192.168.56.102
- Target: Metasploitable 2 192.168.56.101
- Date: 18 May 2026
- Port: 23

## Overview
Connected to the Telnet service on Metasploitable using 
default credentials. No exploit was required — the default 
username and password had never been changed. Verified using 
Wireshark that all credentials were transmitted in plaintext 
across the network.

## Tools
nmap, telnet, Wireshark

## Commands

```bash
nmap -p 23 192.168.56.101
telnet 192.168.56.101
```

```text
Login: msfadmin
Password: msfadmin
```

## Findings
Full shell access was obtained without any exploit. The 
default credentials msfadmin/msfadmin had never been changed. 
Wireshark confirmed that the username and password were 
visible in plaintext in the network capture — anyone on the 
same network could read them.

## What I Learned
Telnet transmits everything in plaintext including credentials. 
It was replaced by SSH decades ago and should never be running 
on any modern system. Default credentials are one of the most 
common real-world vulnerabilities — they must always be changed 
immediately after installation.
