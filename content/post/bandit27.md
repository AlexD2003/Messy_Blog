+++
author = "Alex Domnit"
title = 'Bandit-Level 27'
date = 2024-08-18T21:44:49+03:00
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
There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.\
Clone the repository and find the password for the next level.

### Login
*ssh bandit27@bandit.labs.overthewire.org -p 2220*\
*upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB*

### Doc
{{< highlight html >}}
git (1)              - the stupid content tracker
    clone     Clone a repository into a new directory
{{< /highlight >}}

#### [GIT DOCUMENTATION](https://git-scm.com/doc)

### Solution
This section of bandit focuses souly on teaching you undestanding what a version control system is and how to use it. We are gonna clone the repository using the ssh connection listed above. Modify **localhost** with **localhost:2220** since you want to make the request on port 2220. When you are prompted for a password simply put the password of bandit27.
{{< highlight html >}}
bandit27@bandit:~$ ls
bandit27@bandit:~$ mktemp -d
/tmp/tmp.EL8vk6woto
bandit27@bandit:/tmp/tmp.EL8vk6woto$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit27/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit27-git@localhost's password:
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
{{< /highlight >}}

When you use **clone**, you are gonna get the files of the repository you are trying to clone at a certain state in time. Now watching our directory, there appears to be a new directory, that being the repository we`ve just cloned. **cat** **README** for the password of bandit28.

{{< highlight html >}}
bandit27@bandit:/tmp/tmp.EL8vk6woto$ ls
repo
bandit27@bandit:/tmp/tmp.EL8vk6woto$ cd repo/
bandit27@bandit:/tmp/tmp.EL8vk6woto/repo$ ls
README
bandit27@bandit:/tmp/tmp.EL8vk6woto/repo$ cat README
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
{{< /highlight >}}