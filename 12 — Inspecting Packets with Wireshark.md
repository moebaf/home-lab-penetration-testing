# 06 — Inspecting Packets with Wireshark

## Environment
- Device: Personal machine 192.168.100.88
- Date: 18 May 2026

## Overview
Used Wireshark to monitor network traffic on my own network 
to understand how DNS queries and unencrypted HTTP traffic 
can be observed by anyone on the same network.

## Tool
Wireshark

## Commands

```bash
sudo wireshark
```

```text
dns.qry.name contains youtube
http
```

## Output (DNS capture)

```text
6510  192.168.100.88 -> 192.168.100.1  DNS  Standard query A www.youtube.com
6574  192.168.100.1  -> 192.168.100.88 DNS  Standard query response A www.youtube.com
      CNAME youtube-ui.l.google.com
      A 142.251.216.142
```

The DNS output shows my device communicating with my router 
to resolve youtube.com before connecting to Google's servers.

## Output (HTTP capture)

```text
100   192.168.100.16 -> 41.90.131.26  HTTP  GET /connecttest.txt HTTP/1.1
103   41.90.131.26   -> 192.168.100.16 HTTP  HTTP/1.1 200 OK
2151  192.168.100.88 -> 44.238.29.244 HTTP  POST /login.aspx HTTP/1.1
      (application/x-www-form-urlencoded)
```

The HTTP scan revealed a device on the network submitting a 
login form over unencrypted HTTP. Inspecting the packet showed 
credentials being transmitted in plaintext — visible to anyone 
monitoring the network.

## Findings
DNS queries reveal every website a device visits even without 
seeing the content. Unencrypted HTTP traffic exposes form data 
including login credentials in full plaintext. This demonstrates 
why HTTPS is essential for any website handling user data.

## What I Learned
Wireshark is a powerful tool for understanding what is 
happening on a network. Organizations use it to monitor 
traffic, detect suspicious activity, and identify insecure 
communications. Any website still using HTTP instead of HTTPS 
is putting its users at serious risk.
