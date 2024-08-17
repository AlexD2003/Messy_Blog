+++
author = "Alex Domnit"
title = "Bandit-Level 10"
date = 2024-08-17T22:35:17+03:00
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
The password for the next level is stored in the file **data.txt**, which contains base64 encoded data.

### Login
*ssh bandit10@bandit.labs.overthewire.org -p 2220*\
*FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey*

### Doc
{{< highlight html >}}
base64 (1)           - base64 encode/decode data and print to standard output
    -d, --decode          decode data
{{< /highlight >}}

### Solution
Use the -d flag (decode) on the **data.txt** file by using the **base64** command.
{{< highlight html >}}
bandit10@bandit:~$ base64 -d data.txt
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
{{< /highlight >}}
