# Challenge 
```php 
<?php

$flag ="XXXX"; 

if(empty($_GET['user'])) die(show_source(__FILE__));

$user = ['admin', 'xxoo'];

if($_GET['user'] === $user && $_GET['user'][0] != 'admin'){
    echo $flag;
}
?>
```

# Solution

# Refference
+ [PHP 不同数组之间比较由于整数键截断导致结果相同 ](http://blog.evalbug.com/2015/11/10/different_arrays_compare_indentical_due_to_integer_key_truncation/)