# Challenge
```php 
<h1>hello ctfer!<h1><!--<?php
error_reporting(0);
$flag = "xxxxxxxx";
$secret = "xxxxxxxxxxxxxxxxxxxxxxxxx"; // This secret is 15 characters long for security!
$username = $_POST["username"];
$password = $_POST["password"];
if (!empty($_COOKIE["getmein"])) {
    if (urldecode($username) === "admin" && urldecode($password) != "admin") {
        if ($_COOKIE["getmein"] == md5($secret . urldecode($username . $password))) {
            echo "Congratulations! You are a registered user.\n";
            die ("The flag is ". $flag);
        }
        else {
            die ("Your cookies don't match up! STOP HACKING THIS SITE.");
        }
    }
    else {
        die ("You are not an admin! LEAVE.");
    }
}
setcookie("sample-hash", md5($secret . urldecode("admin" . "admin")), time() + (60 * 60 * 24 * 7));
echo "<h1>hello ctfer!<h1>";
-->
```

# Solution

# Refference
+ [Web: HASH](http://haojiawei.xyz/2017/05/06/%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E5%B0%8F%E7%BB%84WriteUp/#2-Hash%EF%BC%88200%EF%BC%89)