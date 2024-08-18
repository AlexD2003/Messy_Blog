+++
author = "Alex Domnit"
title = 'Bandit-Level 13'
date = 2024-08-18T15:18:52+03:00
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
The password for the next level is stored in **/etc/bandit_pass/bandit14** and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on.

### Login
*ssh bandit13@bandit.labs.overthewire.org -p 2220*\
*FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn*

### Doc
{{< highlight html >}}
ssh (1)              - OpenSSH remote login client
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface] [-b bind_address]
           [-c cipher_spec] [-D [bind_address:]port] [-E log_file]
           [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file]
           [-J destination] [-L address] [-l login_name] [-m mac_spec]
           [-O ctl_cmd] [-o option] [-P tag] [-p port] [-R address]
           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
           destination [command [argument ...]]
       ssh [-Q query_option]
{{< /highlight >}}

#### A bit of backstory
An SSH key is a secure access credential used in the Secure Shell (SSH) protocol. SSH key pairs use public key infrastructure (PKI) technology, the gold standard for digital identity authentication and encryption, to provide a secure and scalable method of authentication.
[Source](https://www.sectigo.com/resource-library/what-is-an-ssh-key)

### Solution
By using **ls**, you can see that there`s a file named **sshkey.private**.
Since it is stated that only bandit14 has access to the flag stored in **/etc/bandit_pass/bandit14** our approach should be making our request as **bandit14**. When **sshing** you can use a key as your login method instead of the text password we were used to until now by specifing the -i flag.
{{< highlight html >}}
bandit13@bandit:~$ ls
sshkey.private
{{< /highlight >}}
Since bandit13, the endpoint we are also on, is hosted on bandit.labs.overthewire.org, we can simply specify localhost as the host and use **ssh** in pair with the key to connect and then just **cat** the respective file.
{{< highlight html >}}
bandit13@bandit:~$ ls
sshkey.private
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit13/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit13/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

!!! You are trying to log into this SSH server with a password on port 2220 from localhost.
!!! Connecting from localhost is blocked to conserve resources.
!!! Please log out and log in again.


      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

....

  Enjoy your stay!

bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
{{< /highlight >}}