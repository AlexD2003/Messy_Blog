+++
author = "Alex Domnit"
title = 'Bandit-Level 7'
date = 2024-08-17T18:16:02+03:00
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
The password for the next level is stored in the file **data.txt** next to the word **millionth**

### Login
*ssh bandit7@bandit.labs.overthewire.org -p 2220*\
*morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj*

### Doc
{{< highlight html >}}
cat (1)              - concatenate files and print on the standard output
grep (1)             - print lines that match patterns
{{< /highlight >}}

### Solution
We can use **cat** to get the contents of the file in question, feed it to **grep** using a pipe **|** with the argument of **millionth** in order for it to spit out the line where the pattern matches.
{{< highlight html >}}
bandit7@bandit:~$ cat data.txt | grep millionth
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
{{< /highlight >}}
