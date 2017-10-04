# Challenge 
```php 
<?php 
include ("config.php");
$conn->query("set names utf8");
function randStr($lenth = 32) {
    $strBase = "1234567890QWERTYUIOPASDFGHJKLZXCVBNMqwertyuiopasdfghjklzxcvbnm";
    $str = "";
    while ($lenth > 0) {
        $str.= substr($strBase, rand(0, strlen($strBase) - 1) , 1);
        $lenth--;
    }
    return $str;
}
if ($install) {
    $sql = "create table `user` (          
    		`id` int(10) unsigned NOT NULL PRIMARY KEY  AUTO_INCREMENT ,          
    		`username` varchar(30) NOT NULL,          
    		`passwd` varchar(32) NOT NULL,          
    		`role` varchar(30) NOT NULL        
    		)ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=latin1 COLLATE=latin1_general_ci ";
    if ($conn->query($sql)) {
        $sql = "insert into `user`(`username`,`passwd`,`role`) values ('admin','" . md5(randStr()) . "','admin')";
        $conn->query($sql);
    }
}
function filter($str) {
    $filter = "/ |\*|#|;|,|is|union|like|regexp|for|and|or|file|--|\||`|&|" . urldecode('%09') . "|" . urldecode("%0a") . "|" . urldecode("%0b") . "|" . urldecode('%0c') . "|" . urldecode('%0d') . "|" . urldecode('%a0') . "/i";
    if (preg_match($filter, $str)) {
        die("you can't input this illegal char!");
    }
    return $str;
}
function show($username) {
    global $conn;
    $sql = "select role from `user` where username ='" . $username . "'";
    $res = $conn->query($sql);
    if ($res->num_rows > 0) {
        echo "$username is " . $res->fetch_assoc() ['role'];
    } else {
        die("Don't have this user!");
    }
}
function login($username, $passwd) {
    global $conn;
    global $flag;
    $username = trim(strtolower($username));
    $passwd = trim(strtolower($passwd));
    if ($username == 'admin') {
        die("you can't login this as admin!");
    }
    $sql = "select * from `user` where username='" . $conn->escape_string($username) . "' and passwd='" . $conn->escape_string($passwd) . "'";
    $res = $conn->query($sql);
    if ($res->num_rows > 0) {
        if ($res->fetch_assoc() ['role'] === 'admin') exit($flag);
    } else {
        echo "sorry,username or passwd error!";
    }
}
function source() {
    highlight_file(__FILE__);
}
$username = isset($_POST['username']) ? filter($_POST['username']) : "";
$passwd = isset($_POST['passwd']) ? filter($_POST['passwd']) : "";
$action = isset($_GET['action']) ? filter($_GET['action']) : "source";
switch ($action) {
    case "source":
        source();
        break;

    case "login":
        login($username, $passwd);
        break;

    case "show":
        show($username);
        break;
}
```

# Refference
+ [MySQL False注入及技巧总结](http://bobao.360.cn/learning/detail/3804.html)