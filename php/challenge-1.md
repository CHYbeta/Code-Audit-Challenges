# Challenge
```php
<?php

/*******************************************************************
 * PHP Challenge 2015
 *******************************************************************
 * Why leave all the fun to the XSS crowd?
 *
 * Do you know PHP?
 * And are you up to date with all its latest peculiarities?
 *
 * Are you sure?
 *
 * If you believe you do then solve this challenge and create an
 * input that will make the following code believe you are the ADMIN.
 * Becoming any other user is not good enough, but a first step.
 *
 * Attention this code is installed on a Mac OS X 10.9 system
 * that is running PHP 5.4.30 !!!
 *
 * TIPS: OS X is mentioned because OS X never runs latest PHP
 *       Challenge will not work with latest PHP
 *       Also challenge will only work on 64bit systems
 *       To solve challenge you need to combine what a normal
 *       attacker would do when he sees this code with knowledge
 *       about latest known PHP quirks
 *       And you cannot bruteforce the admin password directly.
 *       To give you an idea - first half is:
 *          orewgfpeowöfgphewoöfeiuwgöpuerhjwfiuvuger
 *
 * If you know the answer please submit it to info@sektioneins.de
 ********************************************************************/

$users = array(
        "0:9b5c3d2b64b8f74e56edec71462bd97a" ,
        "1:4eb5fb1501102508a86971773849d266",
        "2:facabd94d57fc9f1e655ef9ce891e86e",
        "3:ce3924f011fe323df3a6a95222b0c909",
        "4:7f6618422e6a7ca2e939bd83abde402c",
        "5:06e2b745f3124f7d670f78eabaa94809",
        "6:8e39a6e40900bb0824a8e150c0d0d59f",
        "7:d035e1a80bbb377ce1edce42728849f2",
        "8:0927d64a71a9d0078c274fc5f4f10821",
        "9:e2e23d64a642ee82c7a270c6c76df142",
        "10:70298593dd7ada576aff61b6750b9118"
);

$valid_user = false;

$input = $_COOKIE['user'];
$input[1] = md5($input[1]);

foreach ($users as $user)
{
        $user = explode(":", $user);
        if ($input === $user) {
                $uid = $input[0] + 0;
                $valid_user = true;
        }
}

if (!$valid_user) {
        die("not a valid user\n");
}

if ($uid == 0) {

        echo "Hello Admin How can I serve you today?\n";
        echo "SECRETS ....\n";

} else {
        echo "Welcome back user\n";
}
```
# Solution
此题运行在 PHP 5.4.30 上。
详细解答见： [PHP Challenge 2015 Solution](http://www.sektioneins.de/blog/15-08-03-php_challenge_2015_solution.html)


# Reffenence
+ [PHP Challenge 2015](http://www.sektioneins.de/blog/15-07-31-php_challenge_2015.html)
+ [PHP Challenge 2015 Solution](http://www.sektioneins.de/blog/15-08-03-php_challenge_2015_solution.html)