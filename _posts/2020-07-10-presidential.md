---
title: 'VulnHub - Presidential: 1'
author: Skofos
date: 2020-07-07 00:34:00 +0800
categories: [vulnhub]
tags: [vulnhub, RCE, RCE, LFI, PhpMyAdmin]
toc: false
---

# NetDiscover
yang udah pernah main vulnhub ya taulah langkah pertama ngapain `netdiscover` dullss.
```
 Currently scanning: 10.1.75.0/8   |   Screen View: Unique Hosts                
                                                                                
 295 Captured ARP Req/Rep packets, from 4 hosts.   Total size: 17700            
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.172.2   00:--:--:--:5d:--     52    3120  VMware, Inc.                 
 192.168.172.132 00:--:--:54:--:15     43    2580  VMware, Inc. <-- ini cog                 
 192.168.172.1   00:--:--:--:00:--    193   11580  VMware, Inc.                
 192.168.172.254 00:--:--:ee:--:a4      7     420  VMware, Inc.
 ```
ya terus saya coba semua IPnya dan yah ketemu di bagian kedua

# Nmap
kali ini saya makenya **RustScan** karena biar agak cepet dikit.<br/>
```bash
root@skofos:~/Desktop/vh/presidential# rustscan -T 1500 192.168.172.132 -- -A -sC -sV -oN presidential.scan

     _____           _    _____
    |  __ \         | |  / ____|
    | |__) |   _ ___| |_| (___   ___ __ _ _ __
    |  _  / | | / __| __|\___ \ / __/ _` | '_ \
    | | \ \ |_| \__ \ |_ ____) | (_| (_| | | | |
    |_|  \_\__,_|___/\__|_____/ \___\__,_|_| |_|
    Faster nmap scanning with rust. 
 Automated Decryption Tool - https://github.com/ciphey/ciphey 
 Creator https://github.com/brandonskerritt
WARNING: Your file description limit is lower than selected batch size. Please considering upping this (how to is on the README). NOTE: this may be dangerous and may cause harm to sensitive servers. Automatically reducing Batch Size to match your limit, this process isn't harmful but reduces speed.
Open 80
Open 2082
Starting nmap.
The Nmap command to be run is -A -sC -sV -oN presidential.scan -Pn -vvv -p 80,2082 192.168.172.132
Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-26 13:14 EDT
NSE: Loaded 151 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.00s elapsed
Initiating ARP Ping Scan at 13:14
Scanning 192.168.172.132 [1 port]
Completed ARP Ping Scan at 13:14, 0.04s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 13:14
Scanning votenow.local (192.168.172.132) [2 ports]
Discovered open port 80/tcp on 192.168.172.132
Discovered open port 2082/tcp on 192.168.172.132
Completed SYN Stealth Scan at 13:14, 0.05s elapsed (2 total ports)
Initiating Service scan at 13:14
Scanning 2 services on votenow.local (192.168.172.132)
Completed Service scan at 13:14, 6.03s elapsed (2 services on 1 host)
Initiating OS detection (try #1) against votenow.local (192.168.172.132)
NSE: Script scanning 192.168.172.132.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.27s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.01s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.00s elapsed
Nmap scan report for votenow.local (192.168.172.132)
Host is up, received arp-response (0.00061s latency).
Scanned at 2020-07-26 13:14:19 EDT for 8s

PORT     STATE SERVICE REASON         VERSION
80/tcp   open  http    syn-ack ttl 64 Apache httpd 2.4.6 ((CentOS) PHP/5.5.38)
| http-methods: 
|   Supported Methods: POST OPTIONS GET HEAD TRACE
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.5.38
|_http-title: Ontario Election Services &raquo; Vote Now!
2082/tcp open  ssh     syn-ack ttl 64 OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 06:40:f4:e5:8c:ad:1a:e6:86:de:a5:75:d0:a2:ac:80 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMI84YRpnLBsf4QIK/qH3yifMs49tmZDsZI54D8PxWcmSabJORvZTktzCgH1prKFSr0ZcYLFVGX10vn1v2VFFAXZexf5ApjUaJ0OpWNks3P2BIXMWdugkQ1MdP38lcRBksH6+a84dMeA3qtODwA2CpRkKUlhrLuUjlDiB+YAGDhH6Ku0uiGkYAu9zUSW6OETSmWU7BrixYvWJX0E3jWraevF3lgZCl7i0ou8si0b/iXEuQYX0qO4orcdEp1IAZX0K4j+hH7ssfRvlU9Y8wNDXMFvElbnObmS6Rn3s/y9Zru7RYVWu96dbVN1qWrLvdozuOYENVXwjdDiA2YGzD0Hy9
|   256 e9:e6:3a:83:8e:94:f2:98:dd:3e:70:fb:b9:a3:e3:99 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDil7d9FyzMoBaDDMs6JSw8ZJfGi57gkbnU5ghrPBeYx9Lm4zHcP883tOQjnBthnyrjnvCTHfGlppDsdDmHS1eM=
|   256 66:a8:a1:9f:db:d5:ec:4c:0a:9c:4d:53:15:6c:43:6c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIujGDZtzykOiJxKJi59sA5VD17ENBvz5UM3FdvghWJv
MAC Address: 00:0C:29:54:5D:15 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=7/26%OT=80%CT=%CU=42868%PV=Y%DS=1%DC=D%G=N%M=000C29%TM
OS:=5F1DB9F3%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10D%TI=Z%II=I%TS=A)
OS:OPS(O1=M5B4ST11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B4
OS:ST11NW7%O6=M5B4ST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120)
OS:ECN(R=Y%DF=Y%T=40%W=7210%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%
OS:F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T
OS:5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=
OS:Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF
OS:=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40
OS:%CD=S)

Uptime guess: 0.017 days (since Sun Jul 26 12:50:41 2020)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=263 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE
HOP RTT     ADDRESS
1   0.61 ms votenow.local (192.168.172.132)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 13:14
Completed NSE at 13:14, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.16 seconds
           Raw packets sent: 25 (1.894KB) | Rcvd: 17 (1.366KB)
```
eh gk ada yang menarik nih, yaudahlah saya mencoba liat-liat websitenya gimana.<br/>

<div style="width:100%;height:0px;position:relative;padding-bottom:56.206%;"><iframe src="https://streamable.com/e/t7i2ot?autoplay=1" frameborder="0" width="100%" height="100%" allowfullscreen style="width:100%;height:100%;position:absolute;left:0px;top:0px;overflow:hidden;"></iframe></div>
<br/>
nah itu tuh ada yang menarik `contact@votenow.local`, mencurigakan yaudah saya ke `/etc/hosts/` terus replace IPnya pake nama host `votenow.local`. saya coba lagi masih sama kirain bakal berubah tampilan heheh, yaudah langsung gass aja pake `GoBuster` untuk enumeration directory / file barangkali ada yang menarik.

# Gobuster

eh tambah menarik lagi nih keluarnya.
```bash
root@skofos:~/Desktop/vh/presidential# gobuster dir -u 192.168.172.132 --wordlist /usr/share/dirb/wordlists/common.txt -o presidential.gb -x php,html,bak,php.bak,txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://192.168.172.132
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php,html,bak,php.bak,txt
[+] Timeout:        10s
===============================================================
2020/07/26 13:31:06 Starting gobuster
===============================================================
/.htaccess (Status: 403)
/.htaccess.bak (Status: 403)
/.htaccess.php.bak (Status: 403)
/.htaccess.txt (Status: 403)
/.htaccess.php (Status: 403)
/.htaccess.html (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.html (Status: 403)
/.htpasswd.bak (Status: 403)
/.htpasswd.php.bak (Status: 403)
/.htpasswd.txt (Status: 403)
/.htpasswd.php (Status: 403)
/.hta (Status: 403)
/.hta.txt (Status: 403)
/.hta.php (Status: 403)
/.hta.html (Status: 403)
/.hta.bak (Status: 403)
/.hta.php.bak (Status: 403)
/about.html (Status: 200)
/assets (Status: 301)
/cgi-bin/ (Status: 403) -- ini juga
/cgi-bin/.html (Status: 403) -- awalnya saya kira ini juga
/config.php (Status: 200) -- mencurigakan
/config.php.bak (Status: 200) -- ini juga
/index.html (Status: 200)
/index.html (Status: 200)
===============================================================
2020/07/26 13:31:15 Finished
===============================================================
```
terus saya coba-coba yang menurut saya mencurigakan tadi, eh ketemu dong `/config.php` & `/config.php.bak`. emang awalnya kosong tp pas diliat pake **view-source** di browser, jadi kaya gini
```php
<?php

$dbUser = "votebox";
$dbPass = "casoj3FFASPsbyoRP";
$dbHost = "localhost";
$dbname = "votebox";

?>
```
mungkin ada lagi hal yang menarik kalo `vhost`nya discan lagi. <br/>
kalo pake command `~# gobuster vhost votenow.local -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` bakalan rame, jadi susah untuk nyarinya. lalu saya mencoba pake command `grep "Status: 200"`.
```bash
root@skofos:~/Desktop/vh/presidential# gobuster vhost -u votenow.local --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt | grep "Status: 200"                                                                             
Found: datasafe.votenow.local (Status: 200) [Size: 9503]
```
kan bener bakalan ketemu hal-hal yang mengganjal gitu, lalu tanpa banyak basa basi saya coba aja. dan yeah, btw sebelum ngebuka dipastikan udah disave lagi IP boxya di `/etc/hosts` pke subdomain tadi biar gk dpt Mercusuar" kampretisme.

# Exploit
--soon--
