+++
author = "Alex Domnit"
title = "Bandit-Level 2"
date = 2024-08-17T00:23:19+03:00
draft = false
description = "Writeup"
tags = [
    "CTF",
]
categories = [
    "Overthewire",
    "Bandit",
]

toc = false
+++
# Level goal
The password for the next level is stored in a file called spaces in this filename located in the home directory

### Login
*ssh bandit2@bandit.labs.overthewire.org -p 2220*\
*263JGJPfgU6LtdEvgfWU1XP5yac29mFx*

### Doc
{{< highlight html >}}
ls (1)               - list directory contents
cat (1)              - concatenate files and print on the standard output
{{< /highlight >}}

### Solution
Simply use " " on the file in order to parse the parameter as a string, then get the result using **cat**.
{{< highlight html >}}
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat "spaces in this filename"
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
{{< /highlight >}}
