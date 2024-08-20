+++
author = "Alex Domnit"
title = 'Natas-Level 5'
date = 2024-08-20T14:02:37+03:00
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
*Username: natas5*\
*Password: 0n35PkggAPm2zbEpOU802c0x0Msn1ToK*\
*URL:      http://natas5.natas.labs.overthewire.org*

### Doc
Burp Suite is a proprietary software tool for security assessment and penetration testing of web applications. It software was initially developed in 2003-2006 by Dafydd Stuttard to automate his own security testing needs, after realizing the capabilities of automatable web tools like Selenium.\
[Source.](https://en.wikipedia.org/wiki/Burp_Suite)\
Burp Suite tutorial and usefull guides:\
[Port Swigger.](https://portswigger.net/burp/documentation/desktop/getting-started)\
What is a proxy server?\
A proxy server is a system or router that provides a gateway between users and the internet. Therefore, it helps prevent cyber attackers from entering a private network. It is a server, referred to as an “intermediary” because it goes between end-users and the web pages they visit online.\
When a computer connects to the internet, it uses an IP address. This is similar to your home’s street address, telling incoming data where to go and marking outgoing data with a return address for other devices to authenticate. A proxy server is essentially a computer on the internet that has an IP address of its own.\
[Source.](https://www.fortinet.com/resources/cyberglossary/proxy-server)


### Solution
Logging in for the first time, we get a simple prompt:
{{< highlight html >}}
Access disallowed. You are not logged in
{{< /highlight >}}
This time, we are gonna do pretty much what we`ve been doing until now with the inspect element tool but use the intercept function of **Burp suite** in order to see what is being received and what type of data is being sent. Reloading the page while having the intersect option tuned on should do the trick.
<img src="/img/natas/natas5-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
{{< highlight html >}}
GET / HTTP/1.1
Host: natas5.natas.labs.overthewire.org
Cache-Control: max-age=0
Authorization: Basic bmF0YXM1OjBuMzVQa2dnQVBtMnpiRXBPVTgwMmMweDBNc24xVG9L
Accept-Language: en-US
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Cookie: loggedin=0
Connection: keep-alive
{{< /highlight >}}
There is nothing out of the ordinary in the <strong>HTTP Request</strong> but after a closer look, there is a line <strong>loggedin=0</strong> stored as a cookie. [What is a cookie?](https://www.kaspersky.com/resource-center/definitions/cookies) Changing the value from <strong>false</strong> into <strong>true</strong> and sending the packet will grant us access and gain us the ability to get to the next level.
<img src="/img/natas/natas5-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
<img src="/img/natas/natas5-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>