# 07 — Apache Directory Enumeration

## Environment
- Attacker: Kali Linux 192.168.56.102
- Target: Metasploitable 2 192.168.56.101
- Date: 18 May 2026
- Port: 80

## Overview
Used dirb to brute-force common directory and file names 
against the Apache web server on Metasploitable to discover 
hidden admin panels and sensitive paths that are not linked 
anywhere on the site.

## Tools
dirb, gobuster, nmap, Firefox

## Command

```bash
dirb http://192.168.56.101
```

## Output

```text
START_TIME: Mon May 18 13:58:09 2026
URL_BASE: http://192.168.56.101/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
GENERATED WORDS: 4612

---- Scanning URL: http://192.168.56.101/ ----
+ http://192.168.56.101/cgi-bin/        (CODE:403)
==> DIRECTORY: http://192.168.56.101/dav/
+ http://192.168.56.101/index.php       (CODE:200)
+ http://192.168.56.101/phpinfo.php     (CODE:200|SIZE:48104)
==> DIRECTORY: http://192.168.56.101/phpMyAdmin/
+ http://192.168.56.101/server-status   (CODE:403)
==> DIRECTORY: http://192.168.56.101/test/
==> DIRECTORY: http://192.168.56.101/twiki/

---- Entering directory: http://192.168.56.101/dav/ ----
WARNING: Directory IS LISTABLE. No need to scan it.

---- Entering directory: http://192.168.56.101/phpMyAdmin/ ----
+ http://192.168.56.101/phpMyAdmin/index.php    (CODE:200)
+ http://192.168.56.101/phpMyAdmin/setup/       (CODE:200)
+ http://192.168.56.101/phpMyAdmin/README       (CODE:200)

END_TIME: Mon May 18 13:59:25 2026
DOWNLOADED: 18448 - FOUND: 42
```

## Findings
Discovered multiple admin panels and sensitive directories 
accessible with no authentication required, including 
/phpMyAdmin, /test, /dav, and /twiki. The /dav and several 
phpMyAdmin subdirectories had directory listing enabled, 
meaning their full contents were browsable by anyone.

## Defense Applied

Edited `/etc/apache2/apache2.conf`:

```text
Options -Indexes
ServerTokens Prod
ServerSignature Off
```

Restarted Apache to apply changes.

## What I Learned
Developers often leave admin panels, test directories, and 
sensitive files on servers without linking them publicly. 
Directory enumeration finds them in minutes. Directory listing 
and verbose server headers should always be disabled in any 
production environment.
