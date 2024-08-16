+++
author = "Alex Domnit"
title = "Bandit-Level 1"
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
The password for the next level is stored in a file called **"-"** located in the home directory

### Login
*ssh bandit1@bandit.labs.overthewire.org -p 2220*\
*ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If*

### Doc
{{< highlight html >}}
cat (1)              - concatenate files and print on the standard output
{{< /highlight >}}

### Solution
Using the redirecting operator **<** when can feed **"-"** to **cat** in order to see the contents of the file.
{{< highlight html >}}
bandit1@bandit:~$ cat < -
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
{{< /highlight >}}
