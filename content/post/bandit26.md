+++
author = "Alex Domnit"
title = 'Bandit-Level 26'
date = 2024-08-18T21:40:23+03:00
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
Good job getting a shell! Now hurry and grab the password for bandit27!

### Login
Keep the login connection from bandit25!

### Solution
All you have to do in this level is set the shell as bash from **vi** and then **cat** the password for the next level.
{{< highlight html >}}
:set shell=/bin/bash
:shell
bandit26@bandit:~$ ls
bandit27-do  text.txt
bandit26@bandit:~$ cat text.txt
  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
{{< /highlight >}}
