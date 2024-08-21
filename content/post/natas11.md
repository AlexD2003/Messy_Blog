+++
author = "Alex Domnit"
title = 'Natas-Level 11'
date = 2024-08-21T11:56:21+03:00
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
*Username: natas11*\
*Password: UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk*\
*URL:      http://natas11.natas.labs.overthewire.org*

### [Solution Idea](https://www.youtube.com/@ChrisDale)

### Tools
[CyberChef](https://gchq.github.io/CyberChef/)\
Very usefull site for encrypting and decrypting information. 


### Doc
[What is encryption and decryption?](https://docs.oracle.com/cd/E19047-01/sunscreen151/806-5397/i996724/index.html)\
Encryption is the process by which a readable message is converted to an unreadable form to prevent unauthorized parties from reading it. Decryption is the process of converting an encrypted message back to its original (readable) format. The original message is called the plaintext message. The encrypted message is called the ciphertext message.\
Digital encryption algorithms work by manipulating the digital content of a plaintext message mathematically, using an encryption algorithm and a digital key to produce a ciphertext version of the message. The sender and recipient can communicate securely if the sender and recipient are the only ones who know the key.\
[What is XOR encryption?](https://en.wikipedia.org/wiki/XOR_cipher)\
The XOR operator is extremely common as a component in more complex ciphers. By itself, using a constant repeating key, a simple XOR cipher can trivially be broken using frequency analysis. If the content of any message can be guessed or otherwise known then the key can be revealed. Its primary merit is that it is simple to implement, and that the XOR operation is computationally inexpensive. A simple repeating XOR (i.e. using the same key for xor operation on the whole data) cipher is therefore sometimes used for hiding information in cases where no particular security is required. The XOR cipher is often used in computer malware to make reverse engineering more difficult.\
[What are cookies?🍪](https://policies.google.com/technologies/cookies?hl=en-US#:~:text=Cookies%20are%20small%20pieces%20of,site%20more%20useful%20to%20you.)\
Cookies are small pieces of text sent to your browser by a website you visit. They help that website remember information about your visit, which can both make it easier to visit the site again and make the site more useful to you.
**My bad drawing skills expressed on a whiteboard:** 
<img src="/img/natas/natas11-nustiusadesenez.jpeg" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

### Solution
Let`s begin by taking a close look at the first page. We are promted with a display that reads : **Cookies are protected with XOR encryption** and a prompt to that allows us to set the color of the webpage background (See the cookie doc if you have not yet). In order for the user to not lose his changes to the color of the page, the site uses cookies to store that information. It is clearly stated that the cookie data is encrypted using XOR encryption.

<img src="/img/natas/natas11-1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
If you were to set the background color to any value including the default and take a look at what your browser stores about the page (<strong>Use inspect element</strong>) you will see something like this:

<img src="/img/natas/natas11-2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Same stuff again, we have the source code listed in the right of the prompt. Let`s take a look at what happens behind the courtain and try to understand the code.
<strong>Code in question:</strong>

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
<script>var wechallinfo = { "level": "natas11", "pass": "<censored>" };</script></head>
<?

$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}

$data = loadData($defaultdata);

if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}

saveData($data);



?>

<h1>natas11</h1>
<div id="content">
<body style="background: <?=$data['bgcolor']?>;">
Cookies are protected with XOR encryption<br/><br/>

<?
if($data["showpassword"] == "yes") {
    print "The password for natas12 is <censored><br>";
}

?>

<form>
Background color: <input name=bgcolor value="<?=$data['bgcolor']?>">
<input type=submit value="Set color">
</form>

<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
{{< /highlight >}}

Since this time, the code is pretty convoluted and not intuitive, we will brake it down in segments.

{{< highlight html >}}
$defaultdata = array("showpassword" => "no", "bgcolor" => "#ffffff");
{{< /highlight >}}

It begins by defining a default array, <strong>$defaultdata</strong>, including 2 key pairs <strong>showpassword</strong>, <strong>bgcolor</strong> with values <strong>no</strong> and <strong>#ffffff</strong> respectively.

```
function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0; $i<strlen($text); $i++) {
        $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}
```

This is where the magic happens, it is the function that outputs the result after the application of <strong>XOR encryption</strong>.

{{< highlight html >}}
function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
        $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
        if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
            if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
                $mydata['showpassword'] = $tempdata['showpassword'];
                $mydata['bgcolor'] = $tempdata['bgcolor'];
            }
        }
    }
    return $mydata;
}
{{< /highlight >}}

The loadData function firstly initializes the variable <strong>$myData</strong> with default values and checks if the cookie exists. Afterwards, the script decodes the information from Base64, decrypts it using the <strong>xor_encrypt</strong> function and then decodes the resulting json. Remember this process, it will be usefull. The rest of the code is basic data and function validation.
 
{{< highlight html >}}
function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}
{{< /highlight >}}

This function stores the given array into a cookie🍪.

{{< highlight html >}}
$data = loadData($defaultdata);
{{< /highlight >}}

Basic populating function.

{{< highlight html >}}
if(array_key_exists("bgcolor", $_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}
{{< /highlight >}}

And finally, this is the background color handling function.\
Enough with analizing what happens, let`s try to solve this level.\
Firstly, I will run the json encoding function on the default data in a php compiler in order to see what the initial array looks like in json.\
<img src="/img/natas/natas11-3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
The output of the function is the array : <strong>{"showpassword":"no","bgcolor":"#ffffff"}</strong>.
Remember the cookie from earlier? This is the step where it comes in. Using the <strong>CyberChef</strong> website, I will firstly apply URL decoding on the cookie value (to convert URL specific values to their numeric values) and then apply the steps present in the code in reverse order.\
Now that we have the decoded cookie and we have the output (the json format array) we could find the key since in XOR decryption, knowing 2 values out of the {initial,key,result} gives you access to finding the 3rd unknown value (look at my drawing).

<img src="/img/natas/natas11-5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
Now all there`s left to do since we found a repeating key is get a new cookie coming from the array that has the <strong>no</strong> value changed to <strong>yes</strong>.
<img src="/img/natas/natas11-6.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>
By changing the cookie value with the value we got after this process should pass to the program the modified string and grant us access to the next level.
<img src="/img/natas/natas11-7.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>

