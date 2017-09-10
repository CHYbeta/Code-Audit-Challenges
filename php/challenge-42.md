# Challenge 
```php 
<?php
error_reporting(0);
require_once('flag.php');
if(!isset($_GET['sss'])){
    show_source('index.php');
    die();
}
$sss=$_GET['sss'];
if(strlen($sss)==666){
    if(!preg_match("/[^0-6]/",$sss)){
        eval('$sss='.$sss.';');
        if($sss!=='0x666'){
            if($sss=='0x666'){
                echo $flag;
            }
        }
    }
}
?>
```

# Solutioin

# Refference
+ [HBCTF 第六场比赛 西班牙-web-666-你会喊666吗？](https://si1ence.com/archives/19/)