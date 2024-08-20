+++
author = "Alex Domnit"
title = 'Natas-Level 2'
date = 2024-08-20T11:34:10+03:00
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
*Username: natas2*\
*Password: TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI*\
*URL:      http://natas2.natas.labs.overthewire.org*

### Doc
[What is inspect element?](https://blog.hubspot.com/website/how-to-inspect#:~:text=What%20does%20%E2%80%9Cinspect%20element%E2%80%9D%20mean,live%20inside%20the%20browser%20window.)\
Inspect element is a feature of modern web browsers that enables anyone to view and edit a website’s source code, including its HTML, CSS, JavaScript, and media files. When the source code is modified with the inspect tool, the changes are shown live inside the browser window.\
[How is a webpage structured?](https://slickplan.com/blog/types-of-website-structure#:~:text=The%20website's%20structure%20refers%20to,%2C%20sequential%2C%20matrix%20and%20database.)

### Solution
At the first entry on the page, we are prompted with the text "There is nothing on this page". Surprisingly, this time that is actually correct. After a quick inspection we can see that the dot at the end of the sentance is actually an image. This wouldn`t be very uselfull usually but in the img tag, we can see that the image is locally hosted and we are prompted with the actual location of the image. If you understand how a webpage is structured, you can see that it is possible to navigate the contents if you know what you are looking for.

<img src="/img/natas/natas2-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
There seems to be a file index at <strong>/files</strong>. After navigating to that directory, there seems to be a <strong>users.txt</strong> file present that contains the credentials for <strong>natas3</strong>.

<img src="/img/natas/natas2-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
users.txt:
{{< highlight html >}}
# username:password
alice:BYNdCesZqW
bob:jw2ueICLvT
charlie:G5vCxkVV3m
natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
eve:zo4mJWyNj2
mallory:9urtcpzBmH
{{< /highlight >}}