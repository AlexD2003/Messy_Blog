+++
author = 'Alex Domnit'
title = 'Natas-Level 24'
date = 2024-09-27T16:24:57+03:00
draft = false
description = "Writeup"
tags = [
    "CTF",
    "Completed",
]
categories = [
    "Overthewire",
    "Natas",
]

toc = false
+++

### Login
*Username: natas24*\
*Password: MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd*\
*URL:      http://natas24.natas.labs.overthewire.org*

### Doc
[strcmp](https://www.programiz.com/c-programming/library-function/string.h/strcmp)\
The strcmp() function compares two strings character by character. If the strings are equal, the function returns 0.\
[Common C vulnerabilities.](https://medium.com/@capturethebugs/common-c-vulnerabilities-56ffad22581e)

### Solution
This is another quick solution, the index page seems to take in the input as password and that is about it.

<img src="/img/natas/natas24-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

Let`s take a look at the code section.\
**Relevant snippet :**

{{< highlight html >}}
<?php
    if(array_key_exists("passwd",$_REQUEST)){
        if(!strcmp($_REQUEST["passwd"],"<censored>")){
            echo "<br>The credentials for the next level are:<br>";
            echo "<pre>Username: natas25 Password: <censored></pre>";
        }
        else{
            echo "<br>Wrong!<br>";
        }
    }
    // morla / 10111
?>  
{{< /highlight >}}

This is a 1 to 1 copy of the **natas 23** level, the only difference being that this time, we know nothing about the expected input. In order to get access for the next level, the result of the strcmp function should be 1 <=> strcmp returns 0 which only occurs when the strins are equal.

<img src="/img/natas/natas24-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

The input parameter is treated as a string but, if you try to pass it as an array for example the return of the **strcmp** function is an exception which in turn results 0 and grants access further.

<img src="/img/natas/natas24-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

**Credentials :**\
Username: natas25\
Password: ckELKUWZUfpOv6uxS6M7lXBpBssJZ4Ws


