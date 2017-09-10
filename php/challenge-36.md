# Challenge
```php 
<?php  
$IsMatch= preg_match("/hongya.*ho.*ngya.{4}hongya{3}:\/.\/(.*hongya)/i", trim($_POST["id"]), $match);
if( $IsMatch ){  
  die('Flag: '.$flag);
}
?>
```

# Solution

# Refference
+ [2016第二届陕西省网络空间安全大赛: 0x06 web代码审计(https://www.ycjcl.cc/2016/05/29/di-er-jie-shan-xi-sheng-wang-luo-kong-jian-an-quan-da-sai-writeup/)