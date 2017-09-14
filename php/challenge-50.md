# Challenge 
```php 
<?php
if(isset($_REQUEST['id'])){
	if(preg_match("/'(?:\w*)\W*?[a-z].*(R|ELECT|OIN|NTO|HERE|NION)/i", $_REQUEST['id'])){
		die("Attack detected!!!");
	}
}

$sql = "select * from xxx where id = '{$_GET['id']}'";
echo $sql;
$result = sql_query($_GET['id']);
?>
```

# Solution 

# Refference
