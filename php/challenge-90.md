# Challenge
```php 
 <?php
    if(!isset($_GET['c']) && !isset($_GET['re'])) {
        show_source(__FILE__);
    }

    $selfdir = $_GET['dir'];
    if (!isset($selfdir)) {
      die();
    }
    $secret = '/var/www/html/hackme/' . md5("cetcrce" . $selfdir . $_SERVER['REMOTE_ADDR']);
    @chdir('hackme');
    @mkdir($secret);
    @chdir($secret);

    if (isset($_GET['c']) && strlen($_GET['c']) <= 5) {
        include('waf.php');
        @exec($_GET['c']);
    }elseif(isset($_GET['re'])) {
        @exec('/bin/rm -rf ' . $secret);
        @exec('touch /var/www/html/hackme/index.php');
    }
?>

```

# Solution

# Refference
+ 赛博地球杯工业互联网安全大赛 请关注工控云管理系统的警告记录