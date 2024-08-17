+++
author = "Alex Domnit"
title = 'Bandit-Level 6'
date = 2024-08-16T17:07:33+03:00
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
The password for the next level is stored somewhere on the server and has all of the following properties:\
-owned by user bandit7\
-owned by group bandit6\
-33 bytes in size

### Login
*ssh bandit6@bandit.labs.overthewire.org -p 2220*\
*HWasnPhtq9AVKe0dmk45nxy20cvUa6EG*

### Doc
{{< highlight html >}}
find (1)             - search for files in a directory hierarchy
{{< /highlight >}}

### Solution
Firstly, I searched the home directory since its smaller than the root in case the file was hidden there. After seeing that it was not, I used **find** in root **/** to see that there exists a file with my defined conditions at **/var/lib/dpkg/info/bandit7.password**.
{{< highlight html >}}
bandit6@bandit:~$ find /home -user bandit7 -group bandit6 -size 33c
find: ‘/home/drifter8/chroot’: Permission denied
find: ‘/home/bandit5/inhere’: Permission denied
find: ‘/home/bandit31-git’: Permission denied
find: ‘/home/bandit29-git’: Permission denied
find: ‘/home/ubuntu’: Permission denied
find: ‘/home/bandit30-git’: Permission denied
find: ‘/home/bandit28-git’: Permission denied
find: ‘/home/drifter6/data’: Permission denied
find: ‘/home/bandit27-git’: Permission denied
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c
find: ‘/sys/kernel/tracing’: Permission denied
find: ‘/sys/kernel/debug’: Permission denied
.
.
.
/var/lib/dpkg/info/bandit7.password
.
.
.
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
{{< /highlight >}}
