# Challenge
```php 
extract($_POST);

function goAway() {
    error_log("Hacking attempt.");
    header('Location: /error/');
}

if (!isset($pi) || !is_numeric($pi)) {
    goAway();
}

if (!assert("(int)$pi == 3")) {
    echo "This is not pi.";
} else {
    echo "This might be pi.";
}
```

# Solution
This challenge contains a code injection vulnerability in line 12 that can be used by an attacker to execute arbitrary PHP code on the web server. The operation assert() evaluates PHP code and it contains user input. In line 1, all POST parameters are instantiated as global variables by PHP's built-in function extract(). This can lead to severe problems itself but in this challenge it is only used for a variety of sources. It enables the attacker to set the $pi variable directly via POST Parameter. In line 8 there is a check to verify if the input is numeric and if not the user is redirected to an error page via the goAway() function. However, after the redirect in line 5 the PHP script continues running because there is no exit() call. Thus, user provided PHP code in the pi parameter is always executed, e.g. pi=phpinfo().

# Refference
+ php-security-calendar-2017 Day 10 - Anticipation