+++
author = "Alex Domnit"
title = 'Bandit-Level 29'
date = 2024-08-18T22:14:49+03:00
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
There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.\
Clone the repository and find the password for the next level.

### Login
*ssh bandit29@bandit.labs.overthewire.org -p 2220*\
*4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7*

### Doc
{{< highlight html >}}
git (1)              - the stupid content tracker
    clone     Clone a repository into a new directory
    log       Show commit logs
{{< /highlight >}}

#### [GIT DOCUMENTATION](https://git-scm.com/doc)

### Solution
For this level I\`ll use the same initial process I\`ve used until now.\
Clone->Check what`s in the repo->try to find a password.
{{< highlight html >}}
bandit29@bandit:~$ ls
bandit29@bandit:~$ mktemp -d
/tmp/tmp.DhJWSGlZlb
bandit29@bandit:~$ cd /tmp/tmp.DhJWSGlZlb
bandit29@bandit:/tmp/tmp.DhJWSGlZlb$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit29/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit29/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit29-git@localhost's password:
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (11/11), done.
Receiving objects: 100% (16/16), 1.43 KiB | 734.00 KiB/s, done.
remote: Total 16 (delta 2), reused 0 (delta 0), pack-reused 0
Resolving deltas: 100% (2/2), done.
bandit29@bandit:/tmp/tmp.DhJWSGlZlb$ ls
repo
bandit29@bandit:/tmp/tmp.DhJWSGlZlb$ cd repo/
bandit29@bandit:/tmp/tmp.DhJWSGlZlb/repo$ ls
README.md
bandit29@bandit:/tmp/tmp.DhJWSGlZlb/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
{{< /highlight >}}

As we can see, there`s no password in **production**. **production** refers to a branch of the project. A repo can have multiple branches at the same time. We can find the password for the next level by listing the branches, moving into another branch and checking the state of the **README** there.
{{< highlight html >}}
bandit29@bandit:/tmp/tmp.DhJWSGlZlb/repo$ git branch -r
  origin/HEAD -> origin/master
  origin/dev
  origin/master
  origin/sploits-dev
bandit29@bandit:/tmp/tmp.DhJWSGlZlb/repo$ git checkout dev
Switched to branch 'dev'
bandit29@bandit:/tmp/tmp.DhJWSGlZlb/repo$ git log
commit 5a53eb83a43bac1f0b4e223e469b40ef68a4b6e6 (HEAD -> dev)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:31 2024 +0000

    initial commit of README.md
bandit29@bandit:/tmp/tmp.DhJWSGlZlb/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit29
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
{{< /highlight >}}