# Challenge
```php 
 <html>
<head>
<title>The Wall</title>
</head>
<body>
<?php
include 'flag.php';

if(isset($_REQUEST['life'])&&isset($_REQUEST['soul'])){
    $username = $_REQUEST['life'];
    $password = $_REQUEST['soul'];

    if(!(is_string($username)&&is_string($password))){
        header( "refresh:1;url=login.html");
        die("You are not allowed south of wall");
    }

    $password = md5($password);
    
    include 'connection.php';
    /*CREATE TABLE IF NOT EXISTS users(id INTEGER PRIMARY KEY AUTOINCREMENT,username TEXT,password TEXT,role TEXT)*/

    $message = "";
    if(preg_match('/(union|\|)/i', $username)){
        $message="Dead work alone not in UNIONs"."</br>";
        echo $message;
        die();
    }
    $query = "SELECT * FROM users WHERE username='$username'";
    $result = $pdo->query($query);
    $users = $result->fetchArray(SQLITE3_ASSOC);

    if($users) {
        if($password == $users['password']){
            if($users['role']=="admin"){
                echo "Here is your flag: $flag";
            }elseif($users['role']=="normal"){
                $message = "Welcome, ".$users['users']."</br>";
                $message.= "Unfortunately, only Lord Commander can access flag";
            }else{
                $message = "What did you do?";
            }
        }
        else{
            $message = "Wrong identity for : ".$users['username'];
        }

    }
    else{
        $message = "No such person exists"."<br>";
    }
    echo $message;
}else{
    header( "refresh:1;url=login.html");
    die("Only living can cross The Wall");
}
?>

</body>
</html>
```

# Refference
+ BackdoorCTF 2017:THE-WALL