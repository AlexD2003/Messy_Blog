+++
author = "Alex Domnit"
title = 'Natas-Level 7'
date = 2024-08-20T15:10:38+03:00
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
*Username: natas7*\
*Password: bmg8SvU1LizuWjx3y7xkNERkHxGre0GS*\
*URL:      http://natas7.natas.labs.overthewire.org*

### Doc
[What is php](https://www.w3schools.com/c/c_intro.php)\
PHP is a general-purpose scripting language geared towards web development. It was originally created by Danish-Canadian programmer Rasmus Lerdorf in 1993 and released in 1995. The PHP reference implementation is now produced by the PHP Group.

### Solution
At the first look, there is nothing on this page exept 2 hyperlinks that both point to a page named Home, About respectivly. 
 
<img src="/img/natas/natas7-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

Let`s inspect the page and see what lies behind.

<img src="/img/natas/natas7-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

It seems like the argument after page gets passed to the **index.php** script in order to display the contents of the page which is a big vulnerability since that means that we can run pretty much anything we please. There\`s also a hint that the password we are after is stored in **/etc/natas_webpass/natas8**, same format as in the Bandit wargame if you have followed that. Alltogether, putting the adress of the file in which the password is stored should spit out it\`s contents and grant us access to the next level.

<img src="/img/natas/natas7-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
