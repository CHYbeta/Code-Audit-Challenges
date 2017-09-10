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

# Refference 
+ p神的小秘圈
+ [l3m0n：小密圈专题(1)-配置文件写入问题](http://www.cnblogs.com/iamstudy/articles/config_file_write_vue.html)