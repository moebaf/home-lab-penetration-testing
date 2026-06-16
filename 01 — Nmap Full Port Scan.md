# 01 — Scanning Our Target

## Environment
- Attacker: Kali Linux 192.168.56.102
- Target: Metasploitable 2 192.168.56.101
- Date: 13 May 2026

## Overview
Performed a full service version scan against Metasploitable 2 
to map the attack surface and identify all open ports and 
running services before attempting any exploits.

## Tool
Nmap

## Command

```bash
nmap -sV 192.168.56.101
```

## Output

```text
Starting Nmap 7.98 ( https://nmap.org ) at 2026-05-13 12:18 -0400
Nmap scan report for 192.168.56.101
Host is up (0.0021s latency).

Not shown: 977 closed tcp ports (reset)

PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login       OpenBSD or Solaris rlogind
514/tcp  open  shell       Netkit rshd
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
```

## Findings
23 ports are open out of 1000 scanned. Every open port is a 
potential entry point for an attacker. Several services are 
running outdated versions with known vulnerabilities, including 
vsftpd 2.3.4 and UnrealIRCd — both exploited in later entries.

## What I Learned
Running a version scan with -sV reveals exactly which services 
are running and at what version. This is the foundation of any 
penetration test — you cannot attack what you have not mapped. 
Every open port is a potential attack surface that needs to be 
justified or closed.
