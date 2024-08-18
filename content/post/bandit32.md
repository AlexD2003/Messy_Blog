+++
author = "Alex Domnit"
title = 'Bandit-Level 32'
date = 2024-08-18T22:49:34+03:00
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
After all this git stuff, it’s time for another escape. Good luck!

### Login
*ssh bandit32@bandit.labs.overthewire.org -p 2220*\
*3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K*

### Solution
Since running any command shows $1, which means we could get into another shell. After that, we can see that our associated user is bandit33 wich means we can **cat** the password for the next level.
{{< highlight html >}}
WELCOME TO THE UPPERCASE SHELL
>> $0
$ whoami
bandit33
$ cat /etc/bandit_pass/bandit33
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
{{< /highlight >}}

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.
