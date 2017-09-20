# Challenge 52
```php 
<?php
//by Mawekl
//more challenges coming soon ;)

function validateuser($user)
{
    #Check username
    if(!preg_match('/^[A-Z][a-z]{1,15}$/',$user))
        die('Are you stupid hacker? Don\'t try inject my script!');
}

function validatepass($pass)
{
    #Check password (injection attempt?)
    if(!preg_match('/^[A-Za-z0-9_ ]+$/',$pass))
        header('Location: http://piv.pivpiv.dk/');
        #kick away stupid hacker!
}

function challenge($user, $pass) //Objective: return TRUE
{
    $users = array(
        "Admin" => $_VeryLongPasswords[0],
        "Mawekl" => $_VeryLongPasswords[1]
    );
    validateuser($user);
    validatepass($pass);
    return ($users[$user] == $pass);
}

?>
```

# Solution 

# Refference
+ [How to bypass PHP username and password check in this CTF challenge?](https://security.stackexchange.com/questions/126808/how-to-bypass-php-username-and-password-check-in-this-ctf-challenge)