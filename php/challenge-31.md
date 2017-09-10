# Challenge
```php 
<?php
if(isset($_GET) && !empty($_GET)){
    $url = $_GET['file'];
    $path = 'upload/'.$_GET['path'];
}else{
    show_source(__FILE__);
    exit();
}
 
if(strpos($path,'..') > -1){
    die('SYCwaf!');
}
 
if(strpos($url,'http://127.0.0.1/') === 0){
    file_put_contents($path, file_get_contents($url));
    echo "console.log($path update successed!)";
}else{
    echo "Hello.Geeker";
}
```

# Solution

# Refference
+ [第七季极客大挑战writeup | Syclover Team](http://blog.sycsec.com/?p=894)