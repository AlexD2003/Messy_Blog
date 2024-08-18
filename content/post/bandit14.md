+++
author = "Alex Domnit"
title = 'Bandit-Level 14'
date = 2024-08-18T16:34:58+03:00
draft = false
description = "Writeup"
tags = [
    "CTF",
    "Completed",
]
categories = [
    "Overthewire",
    "Bandit",
]

toc = false
+++

# Level goal
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

### Login
*ssh bandit14@bandit.labs.overthewire.org -p 2220*\
*MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS*

### Doc
{{< highlight html >}}
nc (1)               - arbitrary TCP and UDP connections and listens
usage: nc [-46CDdFhklNnrStUuvZz] [-I length] [-i interval] [-M ttl]
	  [-m minttl] [-O length] [-P proxy_username] [-p source_port]
	  [-q seconds] [-s sourceaddr] [-T keyword] [-V rtable] [-W recvlimit]
	  [-w timeout] [-X proxy_protocol] [-x proxy_address[:port]]
	  [destination] [port]
{{< /highlight >}}

### Solution
The sollution for this level is pretty straight forward, just transmit the current password to **localhost** on port **30000**  using netcat **nc**.
{{< highlight html >}}
bandit14@bandit:~$ nc localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
{{< /highlight >}}
