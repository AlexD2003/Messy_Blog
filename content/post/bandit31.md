+++
author = "Alex Domnit"
title = 'Bandit-Level 31'
date = 2024-08-18T22:40:19+03:00
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
There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.\
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
bandit31@bandit:~$ mktemp -d
/tmp/tmp.yfyYU2PWtI
bandit31@bandit:~$ cd /tmp/tmp.yfyYU2PWtI
bandit31@bandit:/tmp/tmp.yfyYU2PWtI$ git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit31/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit31-git@localhost's password:
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
bandit31@bandit:/tmp/tmp.yfyYU2PWtI$ ls
repo
bandit31@bandit:/tmp/tmp.yfyYU2PWtI$ cd repo/
bandit31@bandit:/tmp/tmp.yfyYU2PWtI/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
{{< /highlight >}}

Seems like this time we need to push a file named **key.txt** to master. That can be achieved by using the structure:

{{< highlight html >}}
git add
git commit
git push
{{< /highlight >}}

Let`s create a file and push it.

{{< highlight html >}}
bandit31@bandit:/tmp/tmp.yfyYU2PWtI/repo$ touch key.txt
bandit31@bandit:/tmp/tmp.yfyYU2PWtI/repo$ echo "May I come in?" > key.txt
bandit31@bandit:/tmp/tmp.yfyYU2PWtI/repo$ cat key.txt
May I come in?
bandit31@bandit:/tmp/tmp.yfyYU2PWtI/repo$ git add .
bandit31@bandit:/tmp/tmp.yfyYU2PWtI/repo$ git commit -m "test"
[master 1aaff63] test
 1 file changed, 1 insertion(+)
bandit31@bandit:/tmp/tmp.yfyYU2PWtI/repo$ git push
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? tes
Please type 'yes', 'no' or the fingerprint: yes
Could not create directory '/home/bandit31/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit31-git@localhost's password:
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 2 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 508 bytes | 508.00 KiB/s, done.
Total 6 (delta 1), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
{{< /highlight >}}
