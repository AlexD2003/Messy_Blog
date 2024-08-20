+++
author = "Alex Domnit"
title = 'Natas-Level 4'
date = 2024-08-20T12:52:52+03:00
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
*Username: natas4*\
*Password: QryZXc2e0zahULdHrtHxzyYkj59kUxLQ*\
*URL:      http://natas4.natas.labs.overthewire.org*

### Tools
From here on out, simply inspecting the page won`t be sufficient in order to solve these level. We will need to make use of some networking tools in order to perform certain actions and exploit vulnerabilities. The primary tool I will be using will be [Burp suite](https://portswigger.net/burp). If you are on linux, install the package correspondent to your distribution. Otherwise follow the download and install page on **portswigger**. As always, and this is valid for any tool, feel free to use whatever software you want or can get your hands onto as long as it has the necessary abilities to complete the task at hand, however, the documentation and solution will be provided in accordance to the tools I use.

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
After connecting to the webpage with the credentials gained from **natas3**, we are prompted with a display that states that our access request is prohibeted and that the site only accepts requests from the address **http://natas5.natas.labs.overthewire.org**.

<img src="/img/natas/natas4-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Altough this seems like a monumental impediment, the only thing stopping us from getting access to the service is modifying our request to be made from the provided adress. Firstly, let`s open the proxy tab and intercept the messages when hitting refresh.

<img src="/img/natas/natas4-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

Now, we`ve gotta modify the **Referer** on the **HTTP Request** in order for it to have the source of **http://natas5.natas.labs.overthewire.org**.

<img src="/img/natas/natas4-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
<strong>After modification:</strong>
<img src="/img/natas/natas4-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Now, pressing <strong>Forward</strong> will redirect the package back to the page as if it was coming from <strong>http://natas5.natas.labs.overthewire.org</strong>, giving us access to the page.
<img src="/img/natas/natas4-5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>