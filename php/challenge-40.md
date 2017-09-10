# Challenge 
```php 
<?php  
    if (isset($_GET['view-source'])) { 
        show_source(__FILE__); 
        exit(); 
    } 
    include('flag.php'); 
    $smile = 1;  
    if (!isset ($_GET['^_^'])) $smile = 0;  
    if (ereg ('\.', $_GET['^_^'])) $smile = 0;  
    if (ereg ('%', $_GET['^_^'])) $smile = 0;  
    if (ereg ('[0-9]', $_GET['^_^'])) $smile = 0;  
    if (ereg ('http', $_GET['^_^']) ) $smile = 0;  
    if (ereg ('https', $_GET['^_^']) ) $smile = 0;  
    if (ereg ('ftp', $_GET['^_^'])) $smile = 0;  
    if (ereg ('telnet', $_GET['^_^'])) $smile = 0;  
    if (ereg ('_', $_SERVER['QUERY_STRING'])) $smile = 0;  
    if ($smile) { 
        if (@file_exists ($_GET['^_^'])) $smile = 0;  
    }  
    if ($smile) { 
        $smile = @file_get_contents ($_GET['^_^']);  
        if ($smile === "(●'◡'●)") die($flag);  
    }  
?>
```

# Solution 

# Refference
+ [ISG CTF 2014：Smile](http://blog.watch0ut.com/2014/09/29/ISG-CTF-2014-Write-up-Smile-Chopper-Cryptobaby-GIF/)