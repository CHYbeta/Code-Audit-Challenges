# Challenge
```php 
<?php
include 'config.php';
foreach(array('_GET','_POST','_COOKIE') as $key){
    foreach($$key as $k => $v){
        if(is_array($v)){
            errorBox("hello,sangebaimao!");
        }else{
            $k[0] !='_'?$$k = addslashes($v):$$k = "";
        }
    }
}
function filter($str){
    $rstr = "";
    for($i=0;$i<strlen($str);$i++){
        if(ord($str[$i])>31 && ord($str[$i])<127){
            $rstr = $rstr.$str[$i];
        }
    }
    $rstr = str_replace('\'','',$rstr);
    return $rstr;
}
if(!empty($message)){
    if(preg_match("/\b(select|insert|update|delete)\b/i",$message)){
        die("hello,sangebaimao!");
    }
    if(filter($message) !== $message){
        die("hello,sangebaimao!");
    }
    $sql="insert guestbook(`message`) value('$message');";
    mysql_query($sql);
    $sql = "select * from guestbook order by id limit 0,5;";
    $result = mysql_query($sql);
    if($result){
        while($row = mysql_fetch_array($result)){
            $id = $row['id'];
            $message = $row['message'];
            echo "|$id|=>|$message|<br/>";
        }
    }
    $message = stripcslashes($message);
    $sql = "delete from guestbook where id=$id or message ='$message';";
    if(!mysql_query($sql)){
        print(mysql_error());
        $sql = "delete from guestbook where id=$id";
        mysql_query($sql);
    };
}
?>
```

# Refference
+ 三个白帽
+ [MySQLi-Error Injection](http://0x48.pw/2016/04/08/0x18/)