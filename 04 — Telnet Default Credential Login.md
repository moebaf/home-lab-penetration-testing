## Overview
Logged into Telnet using default credentials with no exploit needed.

## Target
192.168.56.101 port 23

## Tool
telnet, nmap

## Commands
```bash
nmap -p 23 192.168.56.101
telnet 192.168.56.101
```

## Credentials Used
msfadmin / msfadmin

## Findings
Full shell access gained. Wireshark confirmed credentials 
visible in plaintext on the network.

## Defense
Disabled Telnet by commenting out the telnet line in 
/etc/inetd.conf and restarting inetd.

## What I Learned
Telnet is plaintext — never use it. SSH exists for a reason. 
Default credentials are never changed on misconfigured systems.
