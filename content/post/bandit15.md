+++
author = "Alex Domnit"
title = 'Bandit-Level 15'
date = 2024-08-18T16:40:00+03:00
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
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

### Login
*ssh bandit15@bandit.labs.overthewire.org -p 2220*\
*263JGJPfgU6LtdEvgfWU1XP5yac29mFx*

### Doc
{{< highlight html >}}
openssl (1ssl)       - OpenSSL command line program
man openssl - for all the use cases and flags
{{< /highlight >}}

### Solution
The only difference between this level and the previous one is the fact that the medium is using **SSL\TLS encryption**. We can achieve that by connecting through **openssl** and then posting the current level`s password.
{{< highlight html >}}
bandit15@bandit:~$ openssl s_client -connect localhost:30001
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
---
.
.
.
read R BLOCK
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx --flag
{{< /highlight >}}
