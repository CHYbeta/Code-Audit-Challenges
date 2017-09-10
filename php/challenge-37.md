# Challenge 
```php 
<?php 
error_reporting(0); 
require_once("flag.php"); 
if(!$passwd) 
{ 
  $passwd=$_POST["passwd"]; 
} 
if(!$lockedtxt) 
{ 
  $lockedtxt=$_POST["lockedtxt"]; 
} 
function flag($var) 
{ 
  echo $var; 
} 
if($key) 
{ 
  $unlockedtxt=preg_replace($passwd,$key,$lockedtxt); 
} 
if($unlockedtxt===$flag) 
{ 
  flag("The Correct: "); 
  flag($flag); 
} 
show_source("index.php"); 
// key=flag(\\1) 
 ?>
```

# Solution

# Refference
+ [Web: Preg](http://haojiawei.xyz/2017/05/06/%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E5%B0%8F%E7%BB%84WriteUp/#1-Preg%EF%BC%88200%EF%BC%89)