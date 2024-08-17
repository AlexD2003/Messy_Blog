+++
author = "Alex Domnit"
title = 'Bandit-Level 8'
date = 2024-08-17T21:57:43+03:00
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
The password for the next level is stored in the file data.txt and is the only line of text that occurs only **once**.
### Login
*ssh bandit8@bandit.labs.overthewire.org -p 2220*\
*dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc*

### Doc
{{< highlight html >}}
sort (1)             - sort lines of text files
uniq (1)             - report or omit repeated lines
    -c, --count           prefix lines by the number of occurrences
    -u, --unique          only print unique lines
| -pipe operator
{{< /highlight >}}

### Solution
We pipe the ouput of **sort** (in order to get the identical lines one after the other) and pipe the output to **unique** using the flags -c (counting just to make sure) and -u(unique)
{{< highlight html >}}
bandit8@bandit:~$ sort data.txt | uniq -cu
      1 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
{{< /highlight >}}
