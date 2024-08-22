+++
author = "Alex Domnit"
title = 'Natas-Level 14'
date = 2024-08-22T14:17:44+03:00
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
*Username: natas14*\
*Password: SdqIqBsFcz3yotlNYErZSZwblkm0lrvx*\
*URL:      http://natas14.natas.labs.overthewire.org*

### Doc
[What is SQL?](https://en.wikipedia.org/wiki/SQL)\
Structured Query Language is a domain-specific language used to manage data, especially in a relational database management system. It is particularly useful in handling structured data, i.e., data incorporating relations among entities and variables. 
[What is SQL injection?](https://en.wikipedia.org/wiki/SQL_injection)\
In computing, SQL injection is a code injection technique used to attack data-driven applications, in which malicious SQL statements are inserted into an entry field for execution.

### Solution
This time, we are not prompted with the same login window as until now. We need to log in through a form and there is also the source code present.
<img src="/img/natas/natas14-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
**Source code:**

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
<script>var wechallinfo = { "level": "natas14", "pass": "<censored>" };</script></head>
<body>
<h1>natas14</h1>
<div id="content">
<?php
if(array_key_exists("username", $_REQUEST)) {
    $link = mysqli_connect('localhost', 'natas14', '<censored>');
    mysqli_select_db($link, 'natas14');

    $query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
    if(array_key_exists("debug", $_GET)) {
        echo "Executing query: $query<br>";
    }

    if(mysqli_num_rows(mysqli_query($link, $query)) > 0) {
            echo "Successful login! The password for natas15 is <censored><br>";
    } else {
            echo "Access denied!<br>";
    }
    mysqli_close($link);
} else {
?>

<form action="index.php" method="POST">
Username: <input name="username"><br>
Password: <input name="password"><br>
<input type="submit" value="Login" />
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
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
```
It is not really that hard to understand if you have any experience with <strong>SQL</strong>. The code basically resumes to "ill look for any records that have the username and password as the ones you input for me".\
Afterwards, we need to take a look at another part of the code, that being:
```
if(mysqli_num_rows(mysqli_query($link, $query)) > 0) {
            echo "Successful login! The password for natas15 is <censored><br>";
    } else {
            echo "Access denied!<br>";
    }
```
This part of the executions makes sure that your <strong>SELECT</strong> statement returns something and if it does, ouput the password for the next level.\
This means that all we have to do is manipulate our input such that we select something, we dont care what we do. I taught of going for buzzwords like "admin" and "test" but that didn`t result into any finds. The next logic thing to do is try to execute some sql sequence that is not originally intended. The simplest way to achieve this is to run a <strong>TRUE</strong>=<strong>TRUE</strong> statement wich will always bring something up. It is also worth noting that the <strong>OR</strong> statement returns true if one of the inputs is true.
Simplify the execution:
```
Select * from users where username="variable1" and password="variable2"
```
Since we can\`t do much with variable1, we could try pushing the " or commenting it in regards to out insert in <strong>variable2</strong>. Now let\`s put all of this together in order to run an or statement.
```
variable1=idontcare
variable2=idontcare" or "string"="string
```
This boils down the query to:
```
Select * from users where username="idontcare" and password="idontcare" or "string"="string"
```
Since <strong>"string"</strong> will always be equal to <strong>"string"</strong>, the query should return something.

<img src="/img/natas/natas14-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
<img src="/img/natas/natas14-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Good job!