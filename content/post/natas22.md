+++
author = "Alex Domnit"
title = 'Natas-Level 22'
date = 2024-09-18T15:12:32+03:00
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
*Username: natas22*\
*Password: d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz*\
*URL:      http://natas22.natas.labs.overthewire.org*

### Doc
Burp Suite is a proprietary software tool for security assessment and penetration testing of web applications. It software was initially developed in 2003-2006 by Dafydd Stuttard to automate his own security testing needs, after realizing the capabilities of automatable web tools like Selenium.
[Source.](https://en.wikipedia.org/wiki/Burp_Suite)\
Burp Suite tutorial and usefull guides:\
[Port Swigger.](https://portswigger.net/burp/documentation/desktop/getting-started)\
URL manipulation is one of the easiest attacks to perform. It can be performed by users who are innocently curious or by hackers who are probing for vulnerabilities.
URLs are not just addresses for browsers and servers to use as users go from page to page using links. They are requests from the browser to the server which act as a low-level form of programming. When the browser requests X from the server, the server responds with Y. There is nothing to keep users from entering other "commands" into the browser bar to see what the server will give them back.\
[Source.](https://guides.codepath.com/websecurity/URL-Manipulation)

### Solution
For the first time, the index page is actually empty.

<img src="/img/natas/natas22-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

Let`s take a look at the source code in order to see what happens behind.\
**Relevant code :**

{{< highlight html >}}
if(array_key_exists("revelio", $_GET)) {
    // only admins can reveal the password
    if(!($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1)) {
    header("Location: /");
    }
}
?>
{{< /highlight >}}

{{< highlight html >}}
<?php
    if(array_key_exists("revelio", $_GET)) {
    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas23\n";
    print "Password: <censored></pre>";
    }
?>
{{< /highlight >}}

This will basically spit out the contents present in the second function if there is a parameter revelio present. There is no need for session variable injection since you can just pass the parameter in the URL. Our value should be **?revelio** since **?** marks the beggining of the query string.

<img src="/img/natas/natas22-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

Even tho I added the parameter, there doesn\`t seem to be any response. The next logical step would be to pass it through **Burp** in order to be able to view the http response. I\`ll send the captured packet in the repeater.

<img src="/img/natas/natas22-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

That seems to be enough.\
**Found credentials :**\
Username: natas23\
Password: dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs