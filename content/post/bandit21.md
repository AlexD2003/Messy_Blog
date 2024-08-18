+++
author = "Alex Domnit"
title = 'Bandit-Level 21'
date = 2024-08-18T19:13:40+03:00
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

### Login
*ssh bandit21@bandit.labs.overthewire.org -p 2220*\
*EeoULMCra2q0dSkYj561DX7s1CpBuOBt*

### Doc
[Usefull read if you are new to bash scripting](https://www.geeksforgeeks.org/bash-scripting-introduction-to-bash-and-bash-scripting/)

### Solution
Let\`s inspect the cronjob associated with the next level in **/etc/crond.d**
{{< highlight html >}}
bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir	sysstat
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
{{< /highlight >}}

After using **cat** on **cronjob_bandit22** we can see that a shell script is being run at reboot. Navigate to it and find out it\`s contents.
{{< highlight html >}}
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
{{< /highlight >}}

Basically, all it is doing is redirecting the content of the password for **bandit22** into a file in the temp registry. Check the file and there`s the flag!
{{< highlight html >}}
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
{{< /highlight >}}