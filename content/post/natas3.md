+++
author = "Alex Domnit"
title = 'Natas-Level 3'
date = 2024-08-20T12:15:26+03:00
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
# Solution idea
[Hammer of thor](https://mcpa.github.io/natas/wargame/web/overthewire/2015/09/28/natas03/)

### Login
*Username: natas3*\
*Password: 3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH*\
*URL:      http://natas3.natas.labs.overthewire.org*

### Doc
[What is inspect element?](https://blog.hubspot.com/website/how-to-inspect#:~:text=What%20does%20%E2%80%9Cinspect%20element%E2%80%9D%20mean,live%20inside%20the%20browser%20window.)\
Inspect element is a feature of modern web browsers that enables anyone to view and edit a website’s source code, including its HTML, CSS, JavaScript, and media files. When the source code is modified with the inspect tool, the changes are shown live inside the browser window.\
[How is a webpage structured?](https://slickplan.com/blog/types-of-website-structure#:~:text=The%20website's%20structure%20refers%20to,%2C%20sequential%2C%20matrix%20and%20database.)

### Solution
By inspecting the page, you can see a text snippet that says : 
{{< highlight html >}}
<!-- No more information leaks!! Not even Google will find it this time... -->
{{< /highlight >}}

So that makes me start thinking about search engines. In a nutshell the robots.txt file on a webpage tells search engines which directories not to enter.\
Robots.txt directory:
{{< highlight html >}}
User-agent: *
Disallow: /s3cr3t/
{{< /highlight >}}
After that, by navigating to the s3cr3t tab, the information for **natas4** should be present there.\
/s3cr3t/users.txt:
{{< highlight html >}}
natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
{{< /highlight >}}