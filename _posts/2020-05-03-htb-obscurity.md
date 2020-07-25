---
title: HackTheBox - Obscurity
author: Skofos
date: 2020-05-03 11:33:00 +0800
categories: [hackthebox]
tags: [hackthebox, directory-enumeration, low-privilege-shell]
math: true
---

* Note: **Ini hanya dokumentasi saya, bukan bertujuan untuk para pembaca sekalian, jika para pembaca menemukan yang dicari maka itu hanya kebetulan saja.**

Yap, Obscurity udah **retired** jadi saya bisa menuliskan writeup untuk belajar / kenangan bahwa saya dulu juga pernah berjuang heheh. jadi gass aja disimak, kalo ada yang salah atau kenapa-kenapa bisa [kontak saya](https://skofos.xyz/me/). yaudahlah langsung gass aja boi.

## Nmap

```bash
# nmap -n -v -Pn -p8080 -A --reason -oN nmap.txt 10.10.10.168
...
PORT     STATE SERVICE    REASON         VERSION
8080/tcp open  http-proxy syn-ack ttl 63 BadHTTPServer
| fingerprint-strings:
|   GetRequest:
|     HTTP/1.1 200 OK
|     Date: Tue, 03 Dec 2019 03:50:25
|     Server: BadHTTPServer
|     Last-Modified: Tue, 03 Dec 2019 03:50:25
|     Content-Length: 4171
|     Content-Type: text/html
|     Connection: Closed
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>0bscura</title>
|     <meta http-equiv="X-UA-Compatible" content="IE=Edge">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta name="keywords" content="">
|     <meta name="description" content="">
|     <!--
|     Easy Profile Template
|     http://www.templatemo.com/tm-467-easy-profile
|     <!-- stylesheet css -->
|     <link rel="stylesheet" href="css/bootstrap.min.css">
|     <link rel="stylesheet" href="css/font-awesome.min.css">
|     <link rel="stylesheet" href="css/templatemo-blue.css">
|     </head>
|     <body data-spy="scroll" data-target=".navbar-collapse">
|     <!-- preloader section -->
|     <!--
|     <div class="preloader">
|     <div class="sk-spinner sk-spinner-wordpress">
|   HTTPOptions:
|     HTTP/1.1 200 OK
|     Date: Tue, 03 Dec 2019 03:50:26
|     Server: BadHTTPServer
|     Last-Modified: Tue, 03 Dec 2019 03:50:26
|     Content-Length: 4171
|     Content-Type: text/html
|     Connection: Closed
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>0bscura</title>
|     <meta http-equiv="X-UA-Compatible" content="IE=Edge">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta name="keywords" content="">
|     <meta name="description" content="">
|     <!--
|     Easy Profile Template
|     http://www.templatemo.com/tm-467-easy-profile
|     <!-- stylesheet css -->
|     <link rel="stylesheet" href="css/bootstrap.min.css">
|     <link rel="stylesheet" href="css/font-awesome.min.css">
|     <link rel="stylesheet" href="css/templatemo-blue.css">
|     </head>
|     <body data-spy="scroll" data-target=".navbar-collapse">
|     <!-- preloader section -->
|     <!--
|     <div class="preloader">
|_    <div class="sk-spinner sk-spinner-wordpress">
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: BadHTTPServer
|_http-title: 0bscura
```
