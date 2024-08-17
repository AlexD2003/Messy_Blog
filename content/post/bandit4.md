+++
title = 'Bandit-Level 4'
date = 2024-08-14T17:07:27+03:00
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
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

### Login
*ssh bandit4@bandit.labs.overthewire.org -p 2220*\
*2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ*

### Doc
{{< highlight html >}}
cat (1)              - concatenate files and print on the standard output
{{< /highlight >}}

### Solution
**Cat** files using **operator** until flag is found.
{{< highlight html >}}
bandit4@bandit:~/inhere$ cat < -file00
,Yq�fL����x4Fbandit4@bandit:~/inhere$ cat < -file01
N�bandit4@bandit:~/inhere$ cat < -file02
9�F�p��tk%bandit4@bandit:~/inhere$ cat < -file03
nQy��͍{+R�ZkF*	bandit4@bandit:~/inhere$ cat < -file04

l�]�߯-@gQ÷wzP�ybandit4@bandit:~/inhere$ cat < -file05
�ӻT9��3ˤ��
T՜F�bandit4@bandit:~/inhere$ cat < -file06
�QĹM�p4-��!#g�[?2004hbandit4@bandit:~/inhere$ cat < -file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
{{< /highlight >}}
