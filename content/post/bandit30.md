+++
author = "Alex Domnit"
title = 'Bandit-Level 30'
date = 2024-08-18T22:31:45+03:00
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
There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.\
Clone the repository and find the password for the next level.

### Login
*ssh bandit30@bandit.labs.overthewire.org -p 2220*\
*qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL*

### Doc
{{< highlight html >}}
git (1)              - the stupid content tracker
{{< /highlight >}}

#### [GIT DOCUMENTATION](https://git-scm.com/doc)

### Solution
For this level I\`ll use the same initial process I\`ve used until now.\
Clone->Check what`s in the repo->try to find a password.
{{< highlight html >}}
bandit30@bandit:~$ mktemp -d
/tmp/tmp.bcYsejHEys
bandit30@bandit:~$ cd /tmp/tmp.bcYsejHEys
bandit30@bandit:/tmp/tmp.bcYsejHEys$ git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit30/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit30/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit30-git@localhost's password:
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
bandit30@bandit:/tmp/tmp.bcYsejHEys$ ls
repo
bandit30@bandit:/tmp/tmp.bcYsejHEys$ cd repo/
bandit30@bandit:/tmp/tmp.bcYsejHEys/repo$ ls
README.md
bandit30@bandit:/tmp/tmp.bcYsejHEys/repo$ cat README.md
just an epmty file... muahaha
{{< /highlight >}}

For git, there exist **tags** which can sometimes have information.

{{< highlight html >}}
bandit30@bandit:/tmp/tmp.bcYsejHEys/repo$ git tag
secret
bandit30@bandit:/tmp/tmp.bcYsejHEys/repo$ git show secret
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
{{< /highlight >}}