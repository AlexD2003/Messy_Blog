+++
author = "Alex Domnit"
title = 'Natas-Level 23'
date = 2024-09-18T15:52:06+03:00
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
*Username: natas23*\
*Password: dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs*\
*URL:      http://natas23.natas.labs.overthewire.org*

### Doc
[strstr](https://cplusplus.com/reference/cstring/strstr/)\
A pointer to the first occurrence in str1 of the entire sequence of characters specified in str2, or a null pointer if the sequence is not present in str1.

### Solution
This is another quick solution, the index page seems to take in the input as password and that is about it.

<img src="/img/natas/natas23-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

Let`s take a look at the code section.\
**Relevant snippet :**

{{< highlight html >}}
<?php
    if(array_key_exists("passwd",$_REQUEST)){
        if(strstr($_REQUEST["passwd"],"iloveyou") && ($_REQUEST["passwd"] > 10 )){
            echo "<br>The credentials for the next level are:<br>";
            echo "<pre>Username: natas24 Password: <censored></pre>";
        }
        else{
            echo "<br>Wrong!<br>";
        }
    }
    // morla / 10111
?> 
{{< /highlight >}}

The first entry in the && condition puts on the requirement that the given password should contain **iloveyou**. In php value comparison operations, php tries to take the numerical value in front of the string for example and execute the comparation against the found value. This means that in order for the **&&** operation to return true, we will need to pass a string that :
- Contains **iloveyou**
- Starts with a number grater than 10

**I.e.** 9999iloveyou

<img src="/img/natas/natas23-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
</br>

