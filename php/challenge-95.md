# Challenge 
```php
Please input your name:
 
<html> 
    <head> 
        <title>BabyPHP</title> 
        <meta charset="UTF-8"> 
    </head> 
    <body> 
        <form action="#" method="post"> 
            Please input your name:<input type="text" name="name" /> 
            <input type="submit" value="submit" /> 
        </form> 
    </body> 
</html> 
<?php 
    highlight_file(__FILE__); 
    error_reporting(0); 
    ini_set('open_basedir', '/var/www/html:/tmp'); 
    $file = 'function.php'; 
    $func = isset($_GET['function'])?$_GET['function']:'filters'; 
    call_user_func($func,$_GET); 
    include($file); 
    session_start(); 
    $_SESSION['name'] = $_POST['name']; 
    if($_SESSION['name']=='admin'){ 
        header('location:admin.php'); 
    } 
?> 
```

# Refference
+ XCTF Final 2018 