+++
author = "Alex Domnit"
title = 'Natas-Level 12'
date = 2024-08-21T15:19:34+03:00
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
*Username: natas12*\
*Password: yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB*\
*URL:      http://natas12.natas.labs.overthewire.org*

### Doc
Burp Suite is a proprietary software tool for security assessment and penetration testing of web applications. It software was initially developed in 2003-2006 by Dafydd Stuttard to automate his own security testing needs, after realizing the capabilities of automatable web tools like Selenium.\
[Source.](https://en.wikipedia.org/wiki/Burp_Suite)\
Burp Suite tutorial and usefull guides:\
[Port Swigger.](https://portswigger.net/burp/documentation/desktop/getting-started)\
[What is PHP injection?](https://owasp.org/www-community/vulnerabilities/PHP_Object_Injection)\
PHP Object Injection is an application level vulnerability that could allow an attacker to perform different kinds of malicious attacks, such as Code Injection, SQL Injection, Path Traversal and Application Denial of Service, depending on the context. The vulnerability occurs when user-supplied input is not properly sanitized before being passed to the unserialize() PHP function. Since PHP allows object serialization, attackers could pass ad-hoc serialized strings to a vulnerable unserialize() call, resulting in an arbitrary PHP object(s) injection into the application scope.

### Solution
For the first time, the index page is asking us to upload a file to the service.
<img src="/img/natas/natas12-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
We can again take a look at the source code.\
**Source code:**

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
<script>var wechallinfo = { "level": "natas12", "pass": "<censored>" };</script></head>
<body>
<h1>natas12</h1>
<div id="content">
<?php

function genRandomString() {
    $length = 10;
    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
    $string = "";

    for ($p = 0; $p < $length; $p++) {
        $string .= $characters[mt_rand(0, strlen($characters)-1)];
    }

    return $string;
}

function makeRandomPath($dir, $ext) {
    do {
    $path = $dir."/".genRandomString().".".$ext;
    } while(file_exists($path));
    return $path;
}

function makeRandomPathFromFilename($dir, $fn) {
    $ext = pathinfo($fn, PATHINFO_EXTENSION);
    return makeRandomPath($dir, $ext);
}

if(array_key_exists("filename", $_POST)) {
    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);


        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else {
        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
        } else{
            echo "There was an error uploading the file, please try again!";
        }
    }
} else {
?>

<form enctype="multipart/form-data" action="index.php" method="POST">
<input type="hidden" name="MAX_FILE_SIZE" value="1000" />
<input type="hidden" name="filename" value="<?php print genRandomString(); ?>.jpg" />
Choose a JPEG to upload (max 1KB):<br/>
<input name="uploadedfile" type="file" /><br />
<input type="submit" value="Upload File" />
</form>
<?php } ?>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}
#### Quick summary of the code
The PHP code allows users to upload files by generating a unique file path for each upload, based on a random filename with the .jpg extension. It checks that the file size does not exceed 1KB and provides feedback if the file is too large or if the upload fails. If the file meets the criteria, it is moved to the designated path, and a link to the uploaded file is displayed.
#### What can we do?
As we did in the 5th level, we could use <strong>Burp Suite</strong> in order to intercept the package being sent before it reaches the host and make modifications to that package such that we could <strong>inject</strong> code that is not supposed to be run in normal conditions. Since the service uses PHP, the code we will inject and the file itself will be of php type.\
The simplest solution that comes to mind is once again, to execute the <strong>cat</strong> command on the host with the target of <strong>/etc/natas_webpass/natas13</strong>.
#### What should the content be?
[What is passthru?](https://www.php.net/manual/en/function.passthru.php)\
Utilizing the passthru function in php we should be able to run the target command on the machine and the only thing that remains to do is spit out the result with <strong>echo</strong>.\
Also we should create a jpeg file that is less than 1kb to have it initially pass through.

{{< highlight html >}}
03:35:53 archie@Archie ~ → touch whatever.jpeg
{{< /highlight >}}

Now we need to modify the package and insert the php code we`ve talked about earlier.

<img src="/img/natas/natas12-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
After all of this,we can see that the file has been succesfully uploaded with the <strong>php</strong> extension.

<img src="/img/natas/natas12-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Trying to open this file should execute the payload from passthru and get us in.

<img src="/img/natas/natas12-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>