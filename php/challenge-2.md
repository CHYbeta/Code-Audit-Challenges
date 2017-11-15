# Challenge
```php 
<?php
show_source(__FILE__);
$flag = "xxxx";
if(isset($_GET['time'])){ 
        if(!is_numeric($_GET['time'])){ 
                echo 'The time must be number.'; 
        }else if($_GET['time'] < 60 * 60 * 24 * 30 * 2){ 
                        echo 'This time is too short.'; 
        }else if($_GET['time'] > 60 * 60 * 24 * 30 * 3){ 
                        echo 'This time is too long.'; 
        }else{ 
                sleep((int)$_GET['time']); 
                echo $flag; 
        } 
                echo '<hr>'; 
}
?>
```

# Solution
```
>>> 60 * 60 * 24 * 30 * 2      
5184000                        
>>> 60 * 60 * 24 * 30 * 3      
7776000                        
>>> hex(5184000)               
'0x4f1a00'                     
>>> hex(7776000)               
'0x76a700'                     
```
要是传入普通的数字比如 5184001 ，固然能过掉前两个if判断，但sleep函数就要让你等到天荒地老了。

我们通过GET或者POST传入的参数，是作为字符串保存的。is_numeric()支持普通数字型字符串、科学记数法型字符串、部分支持十六进制0x型字符串。而强制类型转换int，不能正确转换的类型有十六进制型字符串、科学计数法型字符串（部分）。

测试代码：
```php
<?php
show_source(__FILE__);
$temp = $_GET['temp'];
echo (int)$temp;
?> 
```
当传入参数为 `?temp=0x76a701` 之类的十六进制型字符串，
当传入参数如`?temp=0e11`之类的科学计数法型字符串，会输出`0`。
当传入参数如`?temp=4e11`之类的科学计数法型字符串，会输出`4`

所以最后的payload如下：
payload：
```
?time=0x76a701
```

payload2:
```
?time=0e11
```

# Refference
+ [代码小审计 php篇 Challenge 1](http://www.freebuf.com/column/154097.html)
+ [一题关于PHP的CTF](http://www.cnblogs.com/xishaonian/p/6724964.html)