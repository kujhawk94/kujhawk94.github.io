---
layout: post
title:  Simple port scan
date:   2015-08-27
categories:  nmap, network
---
Periodically, I will read something that makes me want to check open ports on this or that server in order to sleep better at night.  I use nmap infrequently and always forget how easy it is to quickly run a port scan on a server.

Scan with “fast mode” – only the top 100 most common open ports:

```
$ nmap -F server.domain.com
 
Starting Nmap 6.47 ( http://nmap.org ) at 2015-08-27 21:41 CDT
Nmap scan report for server.domain.com (50.51.52.153)
Host is up (0.11s latency).
Not shown: 96 filtered ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
443/tcp open  https
 
Nmap done: 1 IP address (1 host up) scanned in 3.19 seconds
```

Additional nmap sites:

- [Top 30 Nmap Command Examples for Sys/Network Admins][1]

[1]: http://www.cyberciti.biz/networking/nmap-command-examples-tutorials/