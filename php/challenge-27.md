# Challenge
index.php
```php 
<?php
include "flag.php";
$a = @$_REQUEST['hello'];
if(!preg_match('/^\w*$/',$a )){
  die('ERROR');
}
eval("var_dump($$a);");
show_source(__FILE__);
?>
```

flag.php:
```php
<?php 
$623b701875cab0973f229b4647b75775 = "flag";
```

# Solution
这里使用了 `preg_match('/^\w*$/',$a )` 进行正则匹配，要求hello的输入必须为数字和字母的组合。
接下来执行了 `eval("var_dump($$a);");` ，但由于过滤了符号无法正常闭合，所以不能像 [Challenge 12](https://github.com/CHYbeta/CTF-Web-Challenge/blob/master/php/challenge-12.md) 那样去构造。

注意var_dump里为`$$a`，可以输出对应的变量的值，但你并不知道flag对应的变量名，若是简单的爆破那就是无头苍蝇。 

php中有一个特殊的变量：[$GLOBALS](http://www.php.net/manual/zh/reserved.variables.globals.php)，它引用全局作用域中可用的全部变量。

所以payload为：
```
index.php?hello=GLOBALS
```

# Refference
+ [百度杯CTF二月Web专场](http://www.au1ge.xyz/2017/02/23/%E7%99%BE%E5%BA%A6%E6%9D%AFctf%E4%BA%8C%E6%9C%88web%E4%B8%93%E5%9C%BAwriteup/)