+++
author = 'Alex Domnit'
title = 'Natas-Level 21'
date = 2024-09-18T12:48:00+03:00
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
*Username: natas21*\
*Password: BPhv63cKE1lkQl04cE5CuFTzXe15NfiH*\
*URL:      http://natas21.natas.labs.overthewire.org*

### Doc
Burp Suite is a proprietary software tool for security assessment and penetration testing of web applications. It software was initially developed in 2003-2006 by Dafydd Stuttard to automate his own security testing needs, after realizing the capabilities of automatable web tools like Selenium.
[Source.](https://en.wikipedia.org/wiki/Burp_Suite)\
Burp Suite tutorial and usefull guides:\
[Port Swigger.](https://portswigger.net/burp/documentation/desktop/getting-started)

### Solution
The index is pretty short, mainly communicating that this website is colocated with another adress and the text **You are logged in as a regular user. Login as an admin to retrieve credentials for natas22**.

<img src="/img/natas/natas21-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

This is the same idea as some other levels before where there is a cookie present that retains the **true** value of you being an admin and the goal of the level is to modify that cookie in order to gain access to the next level.\
Cookie:

<img src="/img/natas/natas21-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

The code snippet where this comes from can be found in the **View sourcecode** section.\
**Code :**

{{< highlight html >}}
function print_credentials() { /* {{{ */
    if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas22\n";
    print "Password: <censored></pre>";
    } else {
    print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas22.";
    }
}
{{< /highlight >}}

Now navigate to the cohost location and login with the same credentials in order to investigate further.

<img src="/img/natas/natas21-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

This seems to be a simple CSS experiment tool. Taking a look at the source code, you can see that this website store each request parameter as a **session key** which means we could store the positive admin value and bring it over to the original website.\
**Source :**

{{< highlight html >}}
if(array_key_exists("submit", $_REQUEST)) {
    foreach($_REQUEST as $key => $val) {
    $_SESSION[$key] = $val;
    }
}

if(array_key_exists("debug", $_GET)) {
    print "[DEBUG] Session contents:<br>";
    print_r($_SESSION);
}

// only allow these keys
$validkeys = array("align" => "center", "fontsize" => "100%", "bgcolor" => "yellow");
$form = "";

$form .= '<form action="index.php" method="POST">';
foreach($validkeys as $key => $defval) {
    $val = $defval;
    if(array_key_exists($key, $_SESSION)) {
    $val = $_SESSION[$key];
    } else {
    $_SESSION[$key] = $val;
    }
    $form .= "$key: <input name='$key' value='$val' /><br>";
}
{{< /highlight >}}

There doesn`t seem to be an imposed limit on the arguments given so we could add more than the default 3. Open Burp Suite and capture the request, adding an **admin true** parameter.

<img src="/img/natas/natas21-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

We can also use the **?debug** parameter as did before in order to confirm the fact that the admin value is now true.

<img src="/img/natas/natas21-5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

The next logical step is to **Ctrl+Shift+i** in order to lookup the current cookie value and import it into the base index.

<img src="/img/natas/natas21-6.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

Now after moving the cookie, the level should be solved.

<img src="/img/natas/natas21-7.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

**Credentials found :**\
Username: natas22\
Password: d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz