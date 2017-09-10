# Challenge
```php 
<?php
$link = mysqli_connect('localhost', 'root', 'root');
mysqli_select_db($link, 'code');

$table = addslashes($_GET['table']);
$sql = "UPDATE `{$table}` 
        SET `username`='admin'
        WHERE id=1";
if(!mysqli_query($link, $sql)) {
    echo(mysqli_error($link));
}
mysqli_close($link);
```

# Refference
+ [关于一个sql注入注入题目的思考](https://paper.seebug.org/216/)