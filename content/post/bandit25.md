+++
author = "Alex Domnit"
title = 'Bandit-Level 25'
date = 2024-08-18T20:54:51+03:00
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
### Sollution idea : [MayADevBe Blog](https://mayadevbe.me/posts/overthewire/bandit/level26/)

# Level goal
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

### Login
*ssh bandit25@bandit.labs.overthewire.org -p 2220*\
*iCi86ttT4KSNe1armKiwbQNmB3YJP3q4*

### What is a shell?
A shell is a type of computer program called a command-line interpreter that lets Linux and Unix users control their operating systems with command-line interfaces. Shells allow users to communicate efficiently and directly with their operating systems. \
[Source](https://www.datacamp.com/blog/what-is-shell)

### Doc
{{< highlight html >}}
ssh (1)              - OpenSSH remote login client
{{< /highlight >}}

#### Step 1.
When we try to log into the shell of bandit26 using ssh, we are instantly being kicked out without being able to do anything. Let`s see what type of shell we are working with

{{< highlight html >}}
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat /usr/bin/show
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
{{< /highlight >}}

The shell present for user bandit26 seems to be showtext. After inspecting it, it seems like all it does is display a text file and then exit right afterwards.
A sollution to this problem would be minimizing your terminal as much as possible such that the text file could not be fully loaded, giving you the oportunity to go into **vi** and execute commands. [vi](https://en.wikipedia.org/wiki/Vi_(text_editor))

| ![image](/img/bandit26-terminal1.jpg)| ![image](/img/bandit26-terminal2.jpg) |
|--------------------------------|--------------------------------|

After you are in **vi** you can just press **:**, put **e** as execute and:

{{< highlight html >}}
cat /etc/bandit_pass/bandit26
s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ
{{< /highlight >}}