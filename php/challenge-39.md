# Challenge 
```php 
<?php
    if (isset($_GET['view-source'])) {
        show_source(__FILE__);
        exit();
    }
    include("./inc.php"); // key & database config
 
    function err($str){ die("<script>alert(\"$str\");window.location.href='./';</script>"); }
 
    $nonce = mt_rand();
 
    extract($_GET); // this is my backdoor 🙂
     
    if (empty($_POST['key'])) {
 
        err("Parameter Missing!");
    }
 
    if ($_POST['key'] !== $key) {
        err("You Are Not Authorized!");
    }
 
    $conn = mysql_connect($host, $user, $pass);
 
    if (!$conn) {
        err("Database Error, Please Contact with GameMaster!");
    }
 
    $query = isset($_POST['query']) ? bin2hex($_POST['query']) : "SELECT flag FROM forward.flag";
    $res = mysql_query($query);
    if (FALSE == $res) {
        err("Database Error, Please Contact with GameMaster!");
    }
 
    $row = mysql_fetch_array($res);
 
    if ($debug) {
        echo "HOST:\t{$host}<br/>";
        echo "USER:\t{$user}<br/>";
    }
 
    echo "<del>FLAG:\t0ctf{</del>" . sha1($nonce . md5($row['flag'])) . "<del>}</del><br/>"; // not real flag
 
    mysql_close($conn);
```

# Solution

# Refference
+ [0ctf 2015 quals – forward](https://blog.squareroots.de/en/2015/03/0ctf-2015-quals-forward-web250/)