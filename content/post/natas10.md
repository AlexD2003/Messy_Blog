+++
author = "Alex Domnit"
title = 'Natas-Level 10'
date = 2024-08-20T17:01:43+03:00
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
*Username: natas10*\
*Password: t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu*\
*URL:      http://natas10.natas.labs.overthewire.org*

### Doc
[What is command injection?](https://owasp.org/www-community/attacks/Command_Injection)\
Command injection is an attack in which the goal is execution of arbitrary commands on the host operating system via a vulnerable application. Command injection attacks are possible when an application passes unsafe user supplied data (forms, cookies, HTTP headers etc.) to a system shell. In this attack, the attacker-supplied operating system commands are usually executed with the privileges of the vulnerable application. Command injection attacks are possible largely due to insufficient input validation.\
This attack differs from Code Injection, in that code injection allows the attacker to add their own code that is then executed by the application. In Command Injection, the attacker extends the default functionality of the application, which execute system commands, without the necessity of injecting code.


### Solution
This is basically the same index file as the one from **natas9**.

<img src="/img/natas/natas10-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
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
<script>var wechallinfo = { "level": "natas10", "pass": "<censored>" };</script></head>
<body>
<h1>natas10</h1>
<div id="content">

For security reasons, we now filter on certain characters<br/><br/>
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
    if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i $key dictionary.txt");
    }
}
?>
</pre>

<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}

Even though this looks very similar to what we did last time, there`s a big difference at play.

{{< highlight html >}}
if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
{{< /highlight >}}

This line of code makes us unable to run multiple commands in the same line in order to get the command injection we hope for. There`s still hope left tho since we can actually take advantage of the grep function by applying grep to a command.

{{< highlight html >}}
a  cat /etc/natas_webpass/natas11
{{< /highlight >}}

<img src="/img/natas/natas10-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">


I had no ideea why after that, there was still no ouput for the password. After looking at the code one more time I realized that right now, I am using grep on **/etc/natas_webpass/natas11/**. Just then I realized that in order for the command injection to work, the **grep** function should be able to find the provided string in the target. Since we have the flag -i (dont keep track of uppercase or lowercase) and 2 more tries, it seems like the letter "e" is included in the flag and it is findable. That`s enough to solve this level.

<img src="/img/natas/natas10-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
