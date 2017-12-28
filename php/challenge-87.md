# Challenge
```php 
if (isset($_POST['password'])) {
    setcookie('hash', md5($_POST['password']));
    header("Refresh: 0");
    exit;
}

$password = '0e836584205638841937695747769655';
if (!isset($_COOKIE['hash'])) {
    echo '<form><input type="password" name="password" />'
       . '<input type="submit" value="Login" ></form >';
    exit;
} elseif (md5($_COOKIE['hash']) == $password) {
    echo 'Login succeeded';
} else {
    echo 'Login failed';
}
```

# Solution
The code snippet suffers from 4 vulnerabilities. First, the type-unsafe comparison operator is used in line 12 to compare the hashed password to a string. The string has the form of the scientific notation and as a result it is interpreted as “zero to the power of X”, which is zero. So if we are able to generate a zero-string for the hashed user input as well, the check compares zero to zero and succeeds. This hashes are called “Magic Hashes” and a Google search reveals that the MD5 hash of the value 240610708 results in the desired properties. The code snippet calculates the MD5 hash of the password twice though, so it is not possible to directly submit the value. Instead you have to exploit the second vulnerability: the first hash is calculated on the server side but stored in a cookie on the client side. Thus the value 240610708 simply has to be directly injected into the password cookie.

There are two more vulnerabilities but they are not relevant for this challenge. First, the comparison of the hashes is vulnerable to timing attacks. To prevent this issue, the PHP function hash_equals() should be used for comparison. Second, the PHP function md5() is used to hash the password. The MD5 algorithm is considered broken and it was not designed for password hashing. Instead a secure password hashing algorithm like BCrypt should be used. It should be noted that passwords also should not be hard coded but separated into a configuration file.

# Refference
+ php-security-calendar-2017 Day 22 - Chimney