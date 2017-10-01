# Challenge
```php 
<?php
/*//设置open_basedir
ini_set("open_basedir", "/home/shawn/www/index/");
 */

if (isset($_GET['file'])) {
	$file = trim($_GET['file']);
} else {
	$file = "main.html";
}

// disallow ip
if (preg_match('/^(http:\/\/)+([^\/]+)/i', $file, $domain)) {
	$domain = $domain[2];
	if (stripos($domain, ".") !== false) {
		die("Hacker");
	}
}

if( @file_get_contents($file)!=''){
echo file_get_contents($file);

}else{

$str=<<<EOF
<html>
<head><title>403 Forbidden</title></head>
<body bgcolor="white">
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.13.5</center>
</body>
</html>
<!-- a padding to disable MSIE and Chrome friendly error page -->
<!-- a padding to disable MSIE and Chrome friendly error page -->
<!-- a padding to disable MSIE and Chrome friendly error page -->
<!-- a padding to disable MSIE and Chrome friendly error page -->
<!-- a padding to disable MSIE and Chrome friendly error page -->
<!-- a padding to disable MSIE and Chrome friendly error page -->
EOF;
echo $str;
}
```

# Refference 
+ 2017 XDCTF web3