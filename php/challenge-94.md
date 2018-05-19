# Challenge 
```php
<?php
$token = sha1($_SERVER['REMOTE_ADDR']);
$dir = '../sandbox/'.$token.'/';
is_dir($dir) ?: mkdir($dir);
is_file($dir.'index.php') ?: file_put_contents($dir.'index.php', str_replace('#SHA1#', $token, file_get_contents('./template')));
switch($_GET['action'] ?: ''){
    case 'go':
        header('Location: http://'.$token.'.sandbox.r-cursive.ml:1337/');
        break;
    case 'reset':
        system('rm -rf '.$dir);
        break;
    default:
        show_source(__FILE__);
}
?>
<style>code{font-family: Segoe Script, Brush Script MT, cursive; font-size: 1.337em;}</style>
```

# Refference
+ Rctf 2018 r-cursive