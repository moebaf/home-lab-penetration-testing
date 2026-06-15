## Overview
Used dirb and gobuster to find hidden directories on the 
Apache web server.

## Target
192.168.56.101 port 80

## Tools
dirb, gobuster, Firefox

## Commands
```bash
dirb http://192.168.56.101
gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirb/common.txt
```

## Findings
Discovered /phpMyAdmin, /dvwa, /mutillidae, /test — all 
accessible without authentication.

## Defense
Disabled directory listing and hidden server version header 
in apache2.conf using Options -Indexes and ServerTokens Prod.

## What I Learned
Developers leave exposed admin panels and test pages. 
Directory enumeration is a critical recon step on any web target.
