+++
author = "Alex Domnit"
title = 'Natas-Level 9'
date = 2024-08-20T16:29:39+03:00
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
*Username: natas9*\
*Password: ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t*\
*URL:      http://natas9.natas.labs.overthewire.org*

### Doc
[What is command injection?](https://owasp.org/www-community/attacks/Command_Injection)\
Command injection is an attack in which the goal is execution of arbitrary commands on the host operating system via a vulnerable application. Command injection attacks are possible when an application passes unsafe user supplied data (forms, cookies, HTTP headers etc.) to a system shell. In this attack, the attacker-supplied operating system commands are usually executed with the privileges of the vulnerable application. Command injection attacks are possible largely due to insufficient input validation.\
This attack differs from Code Injection, in that code injection allows the attacker to add their own code that is then executed by the application. In Command Injection, the attacker extends the default functionality of the application, which execute system commands, without the necessity of injecting code.


### Solution
Again we are prompted with a text field, a submit and the source code. This time the program does something different tho. Instead of asking for a password, it works more like a function in the sense that is searches for something somewhere. 

<img src="/img/natas/natas9-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

Let`s inspect the source code:
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
<script>var wechallinfo = { "level": "natas9", "pass": "<censored>" };</script></head>
<body>
<h1>natas9</h1>
<div id="content">
<form>
Find words containing: <input name=needle><input type=submit name=submit value=Search><br><br>
</form>


Output:
<pre>
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
</pre>

<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}

The logic behind this whole ordeal goes like this :\
-You insert a key\
-The program checks if there is actually a key in the field\
-If it is, the key parameter takes the value of your input\
-Your value get`s passed to **passthru("grep -i $key dictionary.txt");**

That last part was very important and it is gonna be the key to all of this. Because it passes your key as an argument in a grep command that is being run, you can perform something called command injection. It means you are executing commands through a way you are not supposed to. If we use a **;**, we can delimitate commands and make them abe to be run from the same line.\
This all basically adds up to "I will give it a random input, sepparate with ; and then run whatever I want". In this case, it is gonna be the same **cat** of the password file.
{{< highlight html >}}
a ; cat /etc/natas_webpass/natas10
{{< /highlight >}}

Executing this command will solve **natas9**.

<img src="/img/natas/natas9-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
