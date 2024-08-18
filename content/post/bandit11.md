+++
author = "Alex Domnit"
title = 'Bandit-Level 11'
date = 2024-08-17T22:42:32+03:00
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
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by **13** positions.

### Login
*ssh bandit11bandit.labs.overthewire.org -p 2220*\
*dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr*

### Sources
[Ascii code](https://www.ascii-code.com)

### Doc
{{< highlight html >}}
cat (1)              - concatenate files and print on the standard output
{{< /highlight >}}

### Solution
By using **cat** we get the contents of the single file present in the directory. We can see that the present data makes up a string. By simply rotating every character in this string ascendingly by the value 13 we could in theory get the correct answer. I`ve copied the output from **data.txt** into my own terminal, made a c++ program that increases each element in a character array by 13 and then print out the answer.
{{< highlight html >}}
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
{{< /highlight >}}

### C++ code and compilation
```
#include <iostream>
using namespace std;

//Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
//rot by 13

char v[100] = {'G', 'u', 'r', ' ', 'c', 'n', 'f', 'f', 'j', 'b', 'e', 'q', ' ', 'v', 'f', ' ', '7', 'k', '1', '6', 'J', 'A', 'r', 'U', 'V', 'v', '5', 'L', 'x', 'V', 'u', 'J', 'f', 's', 'S', 'V', 'd', 'b', 'b', 't', 'a', 'H', 'G', 'l', 'w', '9', 'D', '4'};

int main() {
    for (int i = 0; i < 48; i++) {
        if (v[i] >= 'A' && v[i] <= 'Z') {
            v[i] = (v[i] - 'A' + 13) % 26 + 'A';
        }
        else if (v[i] >= 'a' && v[i] <= 'z') {
            v[i] = (v[i] - 'a' + 13) % 26 + 'a';
        }
    }
    for (int i = 0; i < 48; i++) {
        cout << v[i];
    }
    return 0;
}
```

{{< highlight html >}}
02:09:33 archie@Archie c++ → g++ decrypt.cpp  -o out
02:10:29 archie@Archie c++ → ./out
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
{{< /highlight >}}
