+++
author = "Alex Domnit"
title = 'Natas-Level 20'
date = 2024-09-10T11:59:40+03:00
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
*Username: natas20*\
*Password: p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw*\
*URL:      http://natas20.natas.labs.overthewire.org*

### Doc
Burp Suite is a proprietary software tool for security assessment and penetration testing of web applications. It software was initially developed in 2003-2006 by Dafydd Stuttard to automate his own security testing needs, after realizing the capabilities of automatable web tools like Selenium.
[Source.](https://en.wikipedia.org/wiki/Burp_Suite)\
Burp Suite tutorial and usefull guides:\
[Port Swigger.](https://portswigger.net/burp/documentation/desktop/getting-started)

### Solution
The index page of **natas 20** gives a quick prompt of **You are logged in as a regular user. Login as an admin to retrieve credentials for natas21**. It is the same premise as for **natas 19**.

<img src="/img/natas/natas20-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

Let`s look at the source code :

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
<script>var wechallinfo = { "level": "natas20", "pass": "<censored>" };</script></head>
<body>
<h1>natas20</h1>
<div id="content">
<?php

function debug($msg) { /* {{{ */
    if(array_key_exists("debug", $_GET)) {
        print "DEBUG: $msg<br>";
    }
}
/* }}} */
function print_credentials() { /* {{{ */
    if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas21\n";
    print "Password: <censored></pre>";
    } else {
    print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas21.";
    }
}
/* }}} */

/* we don't need this */
function myopen($path, $name) {
    //debug("MYOPEN $path $name");
    return true;
}

/* we don't need this */
function myclose() {
    //debug("MYCLOSE");
    return true;
}

function myread($sid) {
    debug("MYREAD $sid");
    if(strspn($sid, "1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM-") != strlen($sid)) {
    debug("Invalid SID");
        return "";
    }
    $filename = session_save_path() . "/" . "mysess_" . $sid;
    if(!file_exists($filename)) {
        debug("Session file doesn't exist");
        return "";
    }
    debug("Reading from ". $filename);
    $data = file_get_contents($filename);
    $_SESSION = array();
    foreach(explode("\n", $data) as $line) {
        debug("Read [$line]");
    $parts = explode(" ", $line, 2);
    if($parts[0] != "") $_SESSION[$parts[0]] = $parts[1];
    }
    return session_encode() ?: "";
}

function mywrite($sid, $data) {
    // $data contains the serialized version of $_SESSION
    // but our encoding is better
    debug("MYWRITE $sid $data");
    // make sure the sid is alnum only!!
    if(strspn($sid, "1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM-") != strlen($sid)) {
    debug("Invalid SID");
        return;
    }
    $filename = session_save_path() . "/" . "mysess_" . $sid;
    $data = "";
    debug("Saving in ". $filename);
    ksort($_SESSION);
    foreach($_SESSION as $key => $value) {
        debug("$key => $value");
        $data .= "$key $value\n";
    }
    file_put_contents($filename, $data);
    chmod($filename, 0600);
    return true;
}

/* we don't need this */
function mydestroy($sid) {
    //debug("MYDESTROY $sid");
    return true;
}
/* we don't need this */
function mygarbage($t) {
    //debug("MYGARBAGE $t");
    return true;
}

session_set_save_handler(
    "myopen",
    "myclose",
    "myread",
    "mywrite",
    "mydestroy",
    "mygarbage");
session_start();

if(array_key_exists("name", $_REQUEST)) {
    $_SESSION["name"] = $_REQUEST["name"];
    debug("Name set to " . $_REQUEST["name"]);
}

print_credentials();

$name = "";
if(array_key_exists("name", $_SESSION)) {
    $name = $_SESSION["name"];
}

?>

<form action="index.php" method="POST">
Your name: <input name="name" value="<?=$name?>"><br>
<input type="submit" value="Change name" />
</form>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}

#### Code breakdown into sections

<strong>The debug function</strong> checks the URL for a debug parameter. It is usually used to aid in development. This will come in handy.

{{< highlight html >}}
function debug($msg) {
    if(array_key_exists("debug", $_GET)) {
        print "DEBUG: $msg<br>";
    }
}
{{< /highlight >}}

<strong>The print credentials function</strong> checks the <strong>$_SESSION</strong> array in order to see if the admin flag of the session is set to 1.
 
{{< highlight html >}}
function print_credentials() {
    if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
        print "You are an admin. The credentials for the next level are:<br>";
        print "<pre>Username: natas21\n";
        print "Password: <censored></pre>";
    } else {
        print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas21.";
    }
}
{{< /highlight >}}

This is really all of the code that can be considered of interest in our vulnerability purpose. The easiest way to exploit this would be session tampering.

<img src="/img/natas/natas20-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Using the debug parameter in the url as mentioned previously, you can can see that the session file it tries to load doesn`t exist even tho this page actually reads. This seems to be the debug session for the default session. As done previously, we can intercept the packet we want to send in order to alter it`s contents.

<img src="/img/natas/natas20-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

#### Burp :

<img src="/img/natas/natas20-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

Press **Ctrl+R** to redirect the packet into the repeater in order to be able to modify the contents of the header and keep sending the modified packet at the same time as you are getting a http response for analysis.\
The header data should be **%0Aadmin 1** which is the URL encoded version of **admin \n 1**. This solution has been used before.

<img src="/img/natas/natas20-5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

Now just try sending the modified header and watch the response. 

<img src="/img/natas/natas20-6.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

That solved it.\
<strong>Password : </strong>BPhv63cKE1lkQl04cE5CuFTzXe15NfiH\
Now letting the proxy through, you can also see the respons on the webpage.

<img src="/img/natas/natas20-7.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

