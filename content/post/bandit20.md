+++
author = "Alex Domnit"
title = 'Bandit-Level 20'
date = 2024-08-18T18:45:25+03:00
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
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think

### Login
*ssh bandit20@bandit.labs.overthewire.org -p 2220*\
*0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO*

### Doc
{{< highlight html >}}
nc (1)               - arbitrary TCP and UDP connections and listens
& - makes a process run in the background
{{< /highlight >}}

### Solution
To solve this level, you need to create a server that listens on an arbitrary port and pipe into it the password for your current level. After that, running the binary present in your home dir **suconnect** should output the password for the next level.
{{< highlight html >}}
bandit20@bandit:~$ echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l localhost 30005 &
[1] 1630689
bandit20@bandit:~$ ./suconnect 30005
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
{{< /highlight >}}
