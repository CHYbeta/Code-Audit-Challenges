# Challenge
```php 
$sanitized = [];

foreach ($_GET as $key => $value) {
    $sanitized[$key] = intval($value);
}

$queryParts = array_map(function ($key, $value) {
    return $key . '=' . $value;
}, array_keys($sanitized), array_values($sanitized));

$query = implode('&', $queryParts);

echo "<a href='/images/size.php?" .
    htmlentities($query) . "'>link</a>";
```

# Solution
There is a cross-site scripting vulnerability in line 13. This bug depends on the fact that the keys of the $_GET array (the GET parameter names) are not sufficiently sanitized in the code. Both the keys and the sanitized GET values are passed to the href attribute of the <a> tag as a concatenated string. The sanitizer htmlentities() is used, however, single quotes are not affected by default by this built-in function. Hence, an attacker is able to perform an XSS attack against the user, for example using the following query parameter that breaks the href attribute and appends an eventhandler with JavaScript code: /?a'onclick%3dalert(1)%2f%2f=c. Note that the payload is within the parameter name, not the parameter value.

# Refference
+ php-security-calendar-2017 Day 12 - String Lights