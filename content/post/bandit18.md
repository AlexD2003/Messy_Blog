+++
author = "Alex Domnit"
title = 'Bandit-Level 18'
date = 2024-08-18T18:31:40+03:00
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
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

### Login
*ssh bandit18@bandit.labs.overthewire.org -p 2220*\
*x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO*

### Doc
{{< highlight html >}}
ssh (1)              - OpenSSH remote login client
{{< /highlight >}}

### Solution
This is a very easy sollution just like the one from the previous level. Since you get logged out right after logging in you can\`t really do much since you can\`t run any commands right? Well you could just pass the output of **echo "cat readme"** into the ssh login and it will cat the required file **readme** right at the start, therefore you don`t need to actually do anything in the terminal yourself.
{{< highlight html >}}
06:32:06 archie@Archie tmp.sZImFQt5fw → echo "cat readme" | ssh bandit18@bandit.labs.overthewire.org -p 2220
Pseudo-terminal will not be allocated because stdin is not a terminal.
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
{{< /highlight >}}
