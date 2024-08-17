+++
author = "Alex Domnit"
title = 'Bandit-Level 5'
date = 2024-08-15T17:07:30+03:00
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
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:\
-human-readable\
-1033 bytes in size\
-not executable

### Login
*ssh bandit5@bandit.labs.overthewire.org -p 2220*\
*4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw*

### Doc
{{< highlight html >}}
find (1)             - search for files in a directory hierarchy
{{< /highlight >}}

### Solution
Make use of the **find** command to search by size and you can see that there`s only 1 file that matches. Even if there were other files, a simple **ls -la** would check the other conditions.
{{< highlight html >}}
bandit5@bandit:~/inhere$ find  -size 1033c
./maybehere07/.file2
bandit5@bandit:~/inhere$ cd ./maybehere07/
bandit5@bandit:~/inhere/maybehere07$ cat .file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
{{< /highlight >}}
