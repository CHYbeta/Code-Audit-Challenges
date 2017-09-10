# Challenge 
```php 
<?php
require("config.php");
$table = $_GET['table']?$_GET['table']:"test";
$table = Filter($table);
mysqli_query($mysqli,"desc `secret_{$table}`") or Hacker();
$sql = "select 'flag{xxx}' from secret_{$table}";
$ret = sql_query($sql);
echo $ret[0];
?>
```

# Refference 
+ [jarvisoj : [61dctf]inject　](http://web.jarvisoj.com:32794/)
+ [chybeta : [61dctf]inject](https://chybeta.github.io/2017/07/05/jarvisoj-web-writeup/#61dctf-inject)