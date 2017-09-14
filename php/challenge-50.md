# Challenge 
```php 
<?php
$flag = "you get the flag.";
if (isset($_REQUEST['id']) && $_REQUEST['id'] === "flag"){
	die("Attack detected!!!");
}

if (isset($_GET['id']) && $_GET['id'] === 'flag'){
	echo $flag;
}

?>
```

# Solution 

# Refference
