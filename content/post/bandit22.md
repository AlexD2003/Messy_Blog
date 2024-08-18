+++
author = "Alex Domnit"
title = 'Bandit-Level 22'
date = 2024-08-18T19:24:13+03:00
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
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

### Login
*ssh bandit22@bandit.labs.overthewire.org -p 2220*\
*tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q*

### Doc
[Usefull read if you are new to bash scripting](https://www.geeksforgeeks.org/bash-scripting-introduction-to-bash-and-bash-scripting/)

### Solution
Same cron setup as in level 21, let`s follow the trace.
{{< highlight html >}}
bandit22@bandit:~$ cd /etc/cron.d
bandit22@bandit:/etc/cron.d$ ls
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir	sysstat
bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
{{< /highlight >}}

This time the script does basically the same thing by copying the password to the level to **/tmp** and then echoing it to the screen. The difference this time is that it utilizes variables, **&**. If we would run the script as it is, it should print out the current password.
{{< highlight html >}}
bandit22@bandit:/usr/bin$ ./cronjob_bandit23.sh
Copying passwordfile /etc/bandit_pass/bandit22 to /tmp/8169b67bd894ddbb4412f91573b38db3
{{< /highlight >}}

Now, you have the option of setting the environment variable **myname** as bandit23 and run the commands from the script to get the password for level 23.
{{< highlight html >}}
bandit22@bandit:/usr/bin$ myname=bandit23
bandit22@bandit:/usr/bin$ echo I am user $myname | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/usr/bin$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
{{< /highlight >}}