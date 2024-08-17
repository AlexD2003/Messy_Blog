+++
author = "Alex Domnit"
title = 'Bandit-Level 11'
date = 2024-08-17T22:42:32+03:00
draft = false
description = "Writeup"
tags = [
    "CTF",
    "Completed",
]
categories = [
    "Overthewire",
    "Bandit",3
]

toc = false
+++

# Level goal
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by **13** positions.

### Login
*ssh bandit2@bandit.labs.overthewire.org -p 2220*\
*263JGJPfgU6LtdEvgfWU1XP5yac29mFx*

### Sources
[MayADevBe Blog](https://mayadevbe.me/posts/overthewire/bandit/level12/) **Cheers!**

### Doc
{{< highlight html >}}
tr (1)               - translate or delete characters
{{< /highlight >}}

### Solution
There are a lot of websites that offer ROT13 encryption/decryption, but sadly there is no build-in ROT13 command in Linux. However, I wanted a solution for the terminal, so I used the tr command for substitution.

The substitution for ROT13 is A->N,…,Z->M. With tr it would be:
{{< highlight html >}}
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
{{< /highlight >}}


