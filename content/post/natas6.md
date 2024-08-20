+++
author = "Alex Domnit"
title = 'Natas-Level 6'
date = 2024-08-20T14:14:57+03:00
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
*Username: natas6*\
*Password: 0RoJwHdSKWFTYR5WuiAewauSuNaBXned*\
*URL:      http://natas6.natas.labs.overthewire.org*

### Doc
[What is C?](https://www.w3schools.com/c/c_intro.php)\
C is a general-purpose programming language created by Dennis Ritchie at the Bell Laboratories in 1972.\
It is a very popular language, despite being old. The main reason for its popularity is because it is a fundamental language in the field of computer science.\
C is strongly associated with UNIX, as it was developed to write the UNIX operating system.

### Solution
This time, there\`s no need for looking at the website\`s guts or packets, there is already a button there that brings you the source of what happens behind and a field that is asking for a secret in order to progress to the next level.

<img src="/img/natas/natas6-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

The source:
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
<script>var wechallinfo = { "level": "natas6", "pass": "<censored>" };</script></head>
<body>
<h1>natas6</h1>
<div id="content">

<?

include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
    }
?>

<form method=post>
Input secret: <input name=secret><br>
<input type=submit name=submit>
</form>

<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}

The **include** argument in C includes libraries and files in order for them to be used in the current context. Looking at the second half part of the source document, we can see that it includes a file at **includes/secret.inc**. Let`s follow the trace to it in order to see what we can find.

<img src="/img/natas/natas6-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
It seems like that file explicitly contains the secret that is being compared against your input in the code snippet. Inputing it into the field at the index of <strong>natas6</strong> should give us access further.

<img src="/img/natas/natas6-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>