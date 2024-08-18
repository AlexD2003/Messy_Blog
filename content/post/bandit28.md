+++
author = "Alex Domnit"
title = 'Bandit-Level 28'
date = 2024-08-18T21:54:46+03:00
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
There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.\
Clone the repository and find the password for the next level.

### Login
*ssh bandit28@bandit.labs.overthewire.org -p 2220*\
*Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN*

### Doc
{{< highlight html >}}
git (1)              - the stupid content tracker
    clone     Clone a repository into a new directory
    log       Show commit logs
{{< /highlight >}}

#### [GIT DOCUMENTATION](https://git-scm.com/doc)

### Solution
Here you\`ll need to know how to use a version control system in order to get information that was available at one point in time but now isn\`t. Firstly, clone the repo to see what we are working with.
{{< highlight html >}}
bandit28@bandit:~$ mktemp -d
/tmp/tmp.1nHId6dQ2k
bandit28@bandit:~$ cd /tmp/tmp.1nHId6dQ2k
bandit28@bandit:/tmp/tmp.1nHId6dQ2k$ git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
bandit28@bandit:/tmp/tmp.1nHId6dQ2k$ ls
repo
bandit28@bandit:/tmp/tmp.1nHId6dQ2k$ cd repo
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ ls
README.md
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
{{< /highlight >}}

As you can see the password seems to not be present this time. `git status` shows you the current start of the project but, while using `git log`, you can see the logs of the changes made. Each log has associated a hash specific to it which helps you **checkout** a **commit** at a previous time. Doing that, we could get the password from the text file before the point of removal.

{{< highlight html >}}
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ git log
commit 8cbd1e08d1879415541ba19ddee3579e80e3f61a (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    fix info leak

commit 73f5d0435070c8922da12177dc93f40b2285e22a
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    add missing data

commit 5f7265568c7b503b276ec20f677b68c92b43b712
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    initial commit of README.md
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ checkout 73f5d0435070c8922da12177dc93f40b2285e22a
checkout: command not found
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ git checkout 73f5d0435070c8922da12177dc93f40b2285e22a
Note: switching to '73f5d0435070c8922da12177dc93f40b2285e22a'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 73f5d04 add missing data
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ ls
README.md
bandit28@bandit:/tmp/tmp.1nHId6dQ2k/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ
{{< /highlight >}}

