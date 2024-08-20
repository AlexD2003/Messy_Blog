+++
author = "Alex Domnit"
title = 'Natas-Level 8'
date = 2024-08-20T15:26:33+03:00
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
*Username: natas8*\
*Password: xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q*\
*URL:      http://natas8.natas.labs.overthewire.org*

### Doc
[What is Binary?](https://en.wikipedia.org/wiki/Binary_number)\
A binary number is a number expressed in the base-2 numeral system or binary numeral system, a method for representing numbers that uses only two symbols for the natural numbers: typically "0" and "1".\
[What is Hex?](https://he3app.com/blogs/understanding-hex-encode-a-guide-for-developers#:~:text=Hex%20Encoding%20is%20a%20popular%20method%20for%20encoding%20data%20that,networks%20or%20stored%20in%20databases.)\
Hex Encoding is a popular method for encoding data that is widely used by developers to improve data security. This encoding method helps to make data more secure by converting binary data into ASCII or Unicode text that can be transmitted over networks or stored in databases.\
[Strrev](https://www.geeksforgeeks.org/strrev-function-in-c/)\
The strrev() function is a built-in function in C and is defined in string.h header file. The strrev() function is used to reverse the given string.\
[What is base64?](https://en.wikipedia.org/wiki/Base64)\
In computer programming, Base64 is a group of binary-to-text encoding schemes that transforms binary data into a sequence of printable characters, limited to a set of 64 unique characters. More specifically, the source binary data is taken 6 bits at a time, then this group of 6 bits is mapped to one of 64 unique characters.

### Solution
At the first sight, this level seems identical to **natas6** in the fact that there`s a secret field and a source link.

<img src="/img/natas/natas8-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
This time tough, there are no inclusions to other files.
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
<script>var wechallinfo = { "level": "natas8", "pass": "<censored>" };</script></head>
<body>
<h1>natas8</h1>
<div id="content">

<?

$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
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

Even though the solution is pretty straight forward, it requires a small bit of C knowledge and how a programming language operates. In the source file, you can see that there is a variable present named **encodedSecret** and following the if statement, the website processes your answer through the **encodeSecrete** function and then compares the outcume with the **encodedSecret** given.\
The encryption process is pretty straight forward. It takes your input, puts it in base 64, reverses the string and then converts the binary code into hex code. By utilizing some free online tools, it is pretty easy to reverse the process starting from the given secret and get the expect output for the code to pass.
#### Step 1 : bin2hex

<img src="/img/natas/natas8-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

#### Step 2 : Reverse the string

<img src="/img/natas/natas8-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

#### Step 3 : Base64 decode

<img src="/img/natas/natas8-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

After all this steps are completed, you can insert the resulted string into the secret field in order to obtain the flag.

<img src="/img/natas/natas8-5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
