+++
author = "Alex Domnit"
title = 'Natas-Level 13'
date = 2024-08-21T16:17:53+03:00
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
*Username: natas13*\
*Password: trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC*\
*URL:      http://natas13.natas.labs.overthewire.org*

### Doc
Burp Suite is a proprietary software tool for security assessment and penetration testing of web applications. It software was initially developed in 2003-2006 by Dafydd Stuttard to automate his own security testing needs, after realizing the capabilities of automatable web tools like Selenium.\
[Source.](https://en.wikipedia.org/wiki/Burp_Suite)\
Burp Suite tutorial and usefull guides:\
[Port Swigger.](https://portswigger.net/burp/documentation/desktop/getting-started)\
[What is PHP injection?](https://owasp.org/www-community/vulnerabilities/PHP_Object_Injection)\
PHP Object Injection is an application level vulnerability that could allow an attacker to perform different kinds of malicious attacks, such as Code Injection, SQL Injection, Path Traversal and Application Denial of Service, depending on the context. The vulnerability occurs when user-supplied input is not properly sanitized before being passed to the unserialize() PHP function. Since PHP allows object serialization, attackers could pass ad-hoc serialized strings to a vulnerable unserialize() call, resulting in an arbitrary PHP object(s) injection into the application scope.

### Solution
So far, this seems like a 1 to 1 copy of **natas12**. The only noticeable difference being that this time, there is a line saying : **For security reasons, we now only accept image files!**.
<img src="/img/natas/natas13-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
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
<script>var wechallinfo = { "level": "natas13", "pass": "<censored>" };</script></head>
<body>
<h1>natas13</h1>
<div id="content">
For security reasons, we now only accept image files!<br/><br/>

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

    $err=$_FILES['uploadedfile']['error'];
    if($err){
        if($err === 2){
            echo "The uploaded file exceeds MAX_FILE_SIZE";
        } else{
            echo "Something went wrong :/";
        }
    } else if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {
        echo "File is not an image";
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
</html>{{< /highlight >}}
#### What is the difference?
This script performs additional checks on the uploaded files. It ensures the file is an image by using <strong>exif_imagetype()</strong>.
#### What can we do?
exif_imagetype() reads the first bytes of an image and checks its signature.[Source.](https://www.php.net/manual/en/function.exif-imagetype.php)\
JPEG files (compressed images) start with an image marker which always contains the marker code hex values FF D8 FF. It does not have a length of the file embedded, thus we need to find JPEG trailer, which is FF D9.[Source.](https://www.file-recovery.com/jpg-signature-format.htm#:~:text=JPEG%20files%20(compressed%20images)%20start,trailer%2C%20which%20is%20FF%20D9.&text=..%20we%20can%20define%20size%20of,file%20which%20is%201342%20bytes.)\
Armed with this at hand we can conclude that the solution is identical to the previous one, the only difference being that we should begin our payload with the signature bytes of a jpeg image. You can find the structure on the source hlink listed above.

#### What should the content be?
[What is passthru?](https://www.php.net/manual/en/function.passthru.php)\
Utilizing the passthru function in php we should be able to run the target command on the machine and the only thing that remains to do is spit out the result with <strong>echo</strong>.\
Also we should create a jpeg file that is less than 1kb to have it initially pass through.

{{< highlight html >}}
03:35:53 archie@Archie ~ → touch whatever.jpg
{{< /highlight >}}

Now we need to modify the package and insert the php code we\`ve talked about earlier.\
Magic numbers are the first bits of a file which uniquely identify the type of file. This makes programming easier because complicated file structures need not be searched in order to identify the file type.
For example, a jpeg file starts with ffd8 ffe0 0010 4a46 4946 0001 0101 0047 ......JFIF.....G ffd8 shows that it's a JPEG file, and ffe0 identify a JFIF type structure. There is an ascii encoding of "JFIF" which comes after a length code, but that is not necessary in order to identify the file. The first 4 bytes do that uniquely.\
[Source.](https://gist.github.com/leommoore/f9e57ba2aa4bf197ebc5)\
We will basically insert the signature of the file into a new file and then append to it the php payload.
{{< highlight html >}}
01:24:24 archie@Archie tmp → echo "GIF87a" >> whatever.jpg
{{< /highlight >}}
Now all there`s left to do is to modify the package in order for the server to percieve it as jpg even tho it is php and then upload it.
<img src="/img/natas/natas13-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
<img src="/img/natas/natas13-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
We can see that the file has been uploaded, now opening it should get us to the source of the file and execute the php payload on the machine.
<img src="/img/natas/natas13-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Do not take into account the gif part as it is part of the signature of the file left as residual. The content after the space is the flag for <strong>natas14</strong>.