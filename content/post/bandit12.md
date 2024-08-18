+++
author = "Alex Domnit"
title = 'Bandit-Level 12'
date = 2024-08-18T14:22:07+03:00
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
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!).

### Login
*ssh bandit12@bandit.labs.overthewire.org -p 2220*\
*7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4*

### Doc
{{< highlight html >}}
ls (1)               - list directory contents
cat (1)              - concatenate files and print on the standard output
{{< /highlight >}}

### Solution
The solution for this level is pretty long and tedious. It`s all about understanding how files are compressed, how to see what a random file in memory actually is and how to get the data behind it.

#### Step 1.
{{< highlight html >}}
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat "spaces in this filename"
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
{{< /highlight >}}
