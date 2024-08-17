+++
author = "Alex Domnit"
title = "Bandit-Level 3"
date = 2024-08-13T00:23:19+03:00
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
The password for the next level is stored in a hidden file in the **inhere** directory.

### Login
*ssh bandit3@bandit.labs.overthewire.org -p 2220*\
*MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx*

### Doc
{{< highlight html >}}
man (1)              - an interface to the system reference manuals
usefull ls flags:
-a, --all
              do not ignore entries starting with .
-A, --almost-all
              do not list implied . and ..
{{< /highlight >}}

### Solution
Use **ls -a** in order to see files precedented by .`s and navigate to the file then cat the file for the flag.
{{< highlight html >}}
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ls -a
.  ..  ...Hiding-From-You
bandit3@bandit:~/inhere$ cat ...Hiding-From-You
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
{{< /highlight >}}
