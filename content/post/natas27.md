+++
author = "Alex Domnit"
title = 'Natas-Level 27'
date = 2024-11-25T16:14:33+02:00
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
*Username: natas27*\
*Password: u3RRffXjysjgwFU6b9xa23i6prmUsYne*\
*URL:      http://natas27.natas.labs.overthewire.org*

### Doc
[SQL tutorial.](https://www.w3schools.com/sql/)\
[SQL string truncation.](https://stackoverflow.com/questions/8731229/sql-server-string-getting-truncated)

### Solution

The index page has only two fields for the username and password, just like some previous levels.

<img src="/img/natas/natas27-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

**Source code:**
```
<?php

// morla / 10111
// database gets cleared every 5 min


/*
CREATE TABLE `users` (
  `username` varchar(64) DEFAULT NULL,
  `password` varchar(64) DEFAULT NULL
);
*/


function checkCredentials($link,$usr,$pass){

    $user=mysqli_real_escape_string($link, $usr);
    $password=mysqli_real_escape_string($link, $pass);

    $query = "SELECT username from users where username='$user' and password='$password' ";
    $res = mysqli_query($link, $query);
    if(mysqli_num_rows($res) > 0){
        return True;
    }
    return False;
}


function validUser($link,$usr){

    $user=mysqli_real_escape_string($link, $usr);

    $query = "SELECT * from users where username='$user'";
    $res = mysqli_query($link, $query);
    if($res) {
        if(mysqli_num_rows($res) > 0) {
            return True;
        }
    }
    return False;
}


function dumpData($link,$usr){

    $user=mysqli_real_escape_string($link, trim($usr));

    $query = "SELECT * from users where username='$user'";
    $res = mysqli_query($link, $query);
    if($res) {
        if(mysqli_num_rows($res) > 0) {
            while ($row = mysqli_fetch_assoc($res)) {
                // thanks to Gobo for reporting this bug!
                //return print_r($row);
                return print_r($row,true);
            }
        }
    }
    return False;
}


function createUser($link, $usr, $pass){

    if($usr != trim($usr)) {
        echo "Go away hacker";
        return False;
    }
    $user=mysqli_real_escape_string($link, substr($usr, 0, 64));
    $password=mysqli_real_escape_string($link, substr($pass, 0, 64));

    $query = "INSERT INTO users (username,password) values ('$user','$password')";
    $res = mysqli_query($link, $query);
    if(mysqli_affected_rows($link) > 0){
        return True;
    }
    return False;
}


if(array_key_exists("username", $_REQUEST) and array_key_exists("password", $_REQUEST)) {
    $link = mysqli_connect('localhost', 'natas27', '<censored>');
    mysqli_select_db($link, 'natas27');


    if(validUser($link,$_REQUEST["username"])) {
        //user exists, check creds
        if(checkCredentials($link,$_REQUEST["username"],$_REQUEST["password"])){
            echo "Welcome " . htmlentities($_REQUEST["username"]) . "!<br>";
            echo "Here is your data:<br>";
            $data=dumpData($link,$_REQUEST["username"]);
            print htmlentities($data);
        }
        else{
            echo "Wrong password for user: " . htmlentities($_REQUEST["username"]) . "<br>";
        }
    }
    else {
        //user doesn't exist
        if(createUser($link,$_REQUEST["username"],$_REQUEST["password"])){
            echo "User " . htmlentities($_REQUEST["username"]) . " was created!";
        }
    }

    mysqli_close($link);
} else {
?>

<form action="index.php" method="POST">
Username: <input name="username"><br>
Password: <input name="password" type="password"><br>
<input type="submit" value="login" />
</form>
<?php } ?>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
```

This time, the code is not really that interesting and there are no evident exploits. All it does is:
- Create a table with a username and password
- It is stated that the database get`s cleared every 5 mins (not really that relevant)
- The defined fields are of type **VARCHAR 64** (!!! Very important for later)
- If there is a username equivalent with the one you state, you are logging into that user with the password you provide (ofc, only if the password matches with what is stored in the DB)
- Otherwise, the user get`s created into the database with your provided password

Our goal would logically be to login into **Natas28** in order to get the flag for the next level.\
In order to explore the account creation feature, I`ve created a "testuser" account with password "a" and afterwards logged into it in order to see if my query went through succesfully.

<img src="/img/natas/natas27-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

<img src="/img/natas/natas27-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

Everything worked as expected.\
Even tho the code doesn`t expose any evident flaws, there is an interesting function that holds a clue.\
To be exact :
```
if($usr != trim($usr)) {
        echo "Go away hacker";
        return False;
    }
```
**trim** — Strip whitespace (or other characters) from the beginning and end of a string. [Source.](https://www.php.net/manual/en/function.trim.php)\
Why is this important? As seen in the doc section, in SQL, if u exceed the given parameter length, your input get\`s truncated.\
For example: if I were to input a string of 64 a\`s followed by however many b\`s, in the database, the stored value for the username would be 64 a\`s.\
Because there is the **Go away hacker** function, we can\`t just put a **Natas28** username followed by some whitespaces in order to override the actual **Natas28** credentials, the caviat here being the fact that we can put an exactly 64 characters long **Natas28** username, whitespaces counted too, and follow it up by whatever string we want and the input will be truncated to **Natas28** without triggering the go away function. 
**Python code:**
```
username="natas28" + " "*57+"a"
with open('/home/archie/pico/username.txt', 'w') as file:
    file.write(username)
```
This is a simple python snippet that writes to a file a natas28 username followed by 57 whitespaces and an a. I`m goin to use this output as the input for the username on the website and put a password of value "test".

<img src="/img/natas/natas27-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

As you can see, the user creation goes through successfully. Now, all there`s left to do is login with the **natas28** username, **still followed by the 57 whitespaces** and with the chosen password which should override the credentials and grant us the flag.

<img src="/img/natas/natas27-5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

And that`s about it.\
**Password for next level:** 1JNwQM1Oi6J6j1k49Xyw7ZN6pXMQInVj