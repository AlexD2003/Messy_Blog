+++
author = "Alex Domnit"
title = 'Bandit-Level 12'
date = 2024-08-18T14:22:07+03:00
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
The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!).

### Login
*ssh bandit12@bandit.labs.overthewire.org -p 2220*\
*7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4*

### Doc
{{< highlight html >}}
file (1)             - determine file type
tar (1)              - an archiving utility
gzip (1)             - compress or expand files
bzip2 (1)            - a block-sorting file compressor, v1.0.8
xxd (1)              - make a hex dump or do the reverse.
{{< /highlight >}}

### Solution
The solution for this level is pretty long and tedious. It`s all about understanding how files are compressed, how to see what a random file in memory actually is and how to get the data behind it.

#### Step 1.
Using **mktemp -d** you can create a temporary directory in the **/tmp** path in order to process certain files or create stuff where you otherwise wouldn`t have permissions to do so. We will move the initial **data.txt** to our new directory and use **xxd** since we know from the start it is a hexdump.
{{< highlight html >}}
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ mktemp -d
/tmp/tmp.esFHBLf5iQ
bandit12@bandit:~$ cp data.txt /tmp/tmp.esFHBLf5iQ/data.txt
bandit12@bandit:~$ cd /tmp/tmp.esFHBLf5iQ
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ cat data.txt | xxd -r > data
{{< /highlight >}}

#### Step 2.
After every decompression, it is important to check what type of data we have on our hands using **file** and apply the appropriate tools until we get to something human readable.

{{< highlight html >}}
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data
data: gzip compressed data, was "data2.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 577
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data data.gz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data.gz  data.txt
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ rm data.txt
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ gzip -d data.gz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data
data: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data data.bz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ bzip2 -d data.bz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data
data: gzip compressed data, was "data4.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data data.gz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ gzip -d data.gz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ tar -xf data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data5.bin  data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ rm data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data5.bin data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ tar -xf data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data6.bin  data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ rm data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data6.bin data.bz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ bzip2 -d data.bz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ tar -xf data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data8.bin  data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ rm data.tar
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ mv data8.bin data.gz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data.gz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ gzip -d data.gz
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ ls
data
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ file data
data: ASCII text
{{< /highlight >}}

#### Step 3. Wrapping up
We can see that we`ve hit an ASCII file. Now using **cat**, the output of the file should be the flag.
{{< highlight html >}}
bandit12@bandit:/tmp/tmp.esFHBLf5iQ$ cat data
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
{{< /highlight >}}
