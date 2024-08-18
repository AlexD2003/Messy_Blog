+++
author = "Alex Domnit"
title = 'Bandit-Level 19'
date = 2024-08-18T18:41:33+03:00
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
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place **(/etc/bandit_pass)**, after you have used the setuid binary.

### Login
*ssh bandit19@bandit.labs.overthewire.org -p 2220*\
*cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8*

### Solution
When using **ls** in homedir, you can see the presence of **bandit20-do**. Running it with the prefix **./** gives you the ability to execute commands with the permissions of the user bandit20. Therefore, you could just cat the contents of **/etc/bandit_pass/bandit20** to get the password needed for the next level.
{{< highlight html >}}
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
{{< /highlight >}}
