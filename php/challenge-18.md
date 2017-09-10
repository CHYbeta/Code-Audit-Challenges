# Challenge

```php 
#GOAL: get password from admin;
error_reporting(0);
require 'db.inc.php';

function clean($str){
    if(get_magic_quotes_gpc()){
        $str=stripslashes($str);
    }
    return htmlentities($str, ENT_QUOTES);
}

$username = @clean((string)$_GET['username']);
$password = @clean((string)$_GET['password']);

$query='SELECT * FROM users WHERE name=\''.$username.'\' AND pass=\''.$password.'\';';
$result=mysql_query($query);
if(!$result || mysql_num_rows($result) < 1){
    die('Invalid password!');
}

$row = mysql_fetch_assoc($result);

echo "Hello ".$row['name']."</br>";
echo "Your password is:".$row['pass']."</br>";
```

# Refference 
+ php4fun第一题
