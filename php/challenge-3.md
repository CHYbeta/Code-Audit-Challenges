# Challenge 
```php
<?php
$str = addslashes($_GET['option']);
$file = file_get_contents('xxxxx/option.php');
$file = preg_replace('|\$option=\'.*\';|', "\$option='$str';", $file);
file_put_contents('xxxxx/option.php', $file);
```
# Refference 
+ p神的小秘圈
+ [l3m0n：小密圈专题(1)-配置文件写入问题](http://www.cnblogs.com/iamstudy/articles/config_file_write_vue.html)