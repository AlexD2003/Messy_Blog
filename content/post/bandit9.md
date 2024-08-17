+++
author = "Alex Domnit"
title = 'Bandit-Level 9'
date = 2024-08-17T22:25:00+03:00
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
The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

### Login
*ssh bandit9@bandit.labs.overthewire.org -p 2220*\
*4CKMh1JI91bUIZZPXDqGanal4xvAg0JM*

### Doc
{{< highlight html >}}
strings (1)          - print the sequences of printable characters in files
grep (1)             - print lines that match patterns
{{< /highlight >}}

### Solution
Use **strings** to only recieve the string values out of the text file, pipe it to **grep** using "=" as the pattern you are searching for.
{{< highlight html >}}
bandit9@bandit:~$ strings data.txt | grep =
=aA"f
\a!;========== the
PWAF=1
	 M),\}=
2Y6=
G';?e=
========== passwordf
========== isc
*=N6
m=</
E=Bty
=sw	
"M1=
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
!&=u&4$
*XA=
{{< /highlight >}}
