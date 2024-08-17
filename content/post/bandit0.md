+++
author = "Alex Domnit"
title = "Bandit-Level 0"
date = 2024-08-11T00:23:19+03:00
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
The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

# Note!
For all bandit level logins, the default route is using ssh.
Format:
{{< highlight html >}}
ssh bandit(level)@bandit.labs.overthewire.org -p 2220
{{< /highlight >}}

### Login
*ssh bandit0@bandit.labs.overthewire.org -p 2220*\
*bandit0*

### Doc
{{< highlight html >}}
ls (1)               - list directory contents
cat (1)              - concatenate files and print on the standard output
{{< /highlight >}}

### Solution
Use "ls" to list the files in the current directory and then "cat" to print the contents of **readme**.
{{< highlight html >}}
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
{{< /highlight >}}
