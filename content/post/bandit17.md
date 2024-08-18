+++
author = "Alex Domnit"
title = 'Bandit-Level 17'
date = 2024-08-18T18:26:37+03:00
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
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

### Login
*ssh -i key.private bandit17@bandit.labs.overthewire.org -p 2220*\
*key.private from level 16*

### Doc
{{< highlight html >}}
diff (1)             - compare files line by line
{{< /highlight >}}

### Solution
Very simple level overall. When using **ls** you see 2 files present, **passwords.old** and **passwords.new**. Just use **diff** between these 2 files and see the line that was changed from old to new, that being the password for the next level.
{{< highlight html >}}
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp
{{< /highlight >}}
