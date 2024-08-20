+++
author = "Alex Domnit"
title = 'Natas-Level 0'
date = 2024-08-19T22:46:42+03:00
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

# Introduction
This is the introductory level for the "Natas" wargame hosted by [Overthewire](https://overthewire.org/wargames/natas/)

### What is Natas?
Natas teaches the basics of serverside web-security.

Each level of natas consists of its own website located at http://natasX.natas.labs.overthewire.org, where X is the level number. There is no SSH login. To access a level, enter the username for that level (e.g. natas0 for level 0) and its password.

Each level has access to the password of the next level. Your job is to somehow obtain that next password and level up. All passwords are also stored in /etc/natas_webpass/. E.g. the password for natas5 is stored in the file /etc/natas_webpass/natas5 and only readable by natas4 and natas5.
[Source](https://overthewire.org/wargames/natas/)

There will be no level goals for this wargame, just get in and mess around.

### Login
*Username: natas0*\
*Password: natas0*\
*URL:      http://natas0.natas.labs.overthewire.org*

### Doc
[What is inspect element?](https://blog.hubspot.com/website/how-to-inspect#:~:text=What%20does%20%E2%80%9Cinspect%20element%E2%80%9D%20mean,live%20inside%20the%20browser%20window.)\
Inspect element is a feature of modern web browsers that enables anyone to view and edit a website’s source code, including its HTML, CSS, JavaScript, and media files. When the source code is modified with the inspect tool, the changes are shown live inside the browser window.

### Solution
Let`s connect to the provided URL with the given credentials. Immediatly we are hit with a prompt saying that the password for the next level is located in this webpage.

![image](/img/natas/natas0-1.png)

The first thing you want to do when you don\`t know what to do in order to progress further is to inspect the webpage you suspect could have a clue and sniff around the guts of the page to see what you can find. In this case, because this is the first level, the password is just there and that\`s it! Use it for natas1.

<img src="/img/natas/natas0-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>