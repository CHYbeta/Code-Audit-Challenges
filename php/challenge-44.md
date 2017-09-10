# Challenge
```php 
<?php
session_start();
require('config.php');
foreach($_POST as $key => $value){
        $$key = (trim((string)$value) != '')?trim((string)$value):null;
}
$conn = mysql_connect($server,$dbusr,$dbpwd);
if($conn === false){
        die('Connect Failed.');
}
mysql_select_db($dbname);
if(isset($_SESSION['uid'])){
        $token = getToken($_SESSION['uid']);
        
        echo "Your token is {$token}<br>\n";
        
}elseif(isset($customid) && isset($password)){
        if(strlen($customid) < 6 || strlen($password) < 6 || strlen($customid) > 11 || strlen($password) > 30){
                die('1:Error.');
        }
        
        if(!is_numeric($customid)){
                die('2:Error.');
        }
        
        register($customid, $password);
        
        $_SESSION['uid'] = getuid($customid);
        
        header('location: ./index.php?'.time());
}else{
        echo <<<EOD
<form action="index.php" method="post">
        CustomID: <input type="text" name="customid" maxlength="11"><br>
        Password: <input type="password" name="password" maxlength="30"><br>
        <input type="submit" value="register"><br>
</form>
EOD;
}
mysql_close($conn);
function register($customid, $password){
        $password = md5($password);
        $token = md5(mt_rand());
        mysql_query("insert into z_users(`customid`, `password`) values('{$customid}','{$password}')");
        $result = mysql_query("SELECT LAST_INSERT_ID()");
        $rows=mysql_fetch_row($result);
        $uid = $rows[0];
        mysql_query("insert into z_extra(`uid`, `data`) values('{$uid}','{$token}')");
}
function getuid($customid){
        $result = mysql_query("select * from z_users where customid = '{$customid}' order by id desc");
        if($result){
                $row = mysql_fetch_array($result);
                return $row['id'];
        }
        return 0;
}
function getToken($id){
        $result = mysql_query("SELECT data from z_extra where uid = {$id}");
        $rows = mysql_fetch_row($result);
        return $rows[0];
}
echo "<h4>Source: </h4>";
show_source(__FILE__);
?>
```

# Solution 
# Refference
+ [HappyCTF WriteUp: Register](https://mochazz.github.io/2017/07/04/happyctf/#Register)