# Challenge 
```php 
<?php
error_reporting(0);
ini_set('open_basedir', '/var/www/html');

function autoload($page) {
    if (stripos($_SERVER['QUERY_STRING'], 'flag') > 0) {
      die('no flag flag flag flag !');
    }

    if (stripos($_SERVER['QUERY_STRING'], 'uploaded') > 0) {
      die('no uploaded uploaded uploaded uploaded !');
    }

    if (stripos($_SERVER['QUERY_STRING'], '://f') > 0) {
      die('no ://f ://f ://f');
    }

    if (stripos($_SERVER['QUERY_STRING'], 'ata') > 0) {
      die('no ata ata ata');
    }

    if (stripos($_SERVER['QUERY_STRING'], '0') > 0) {
      die('no 0 0 0');
    }

    if(file_exists("./includes/$page.php")) {
        include "./includes/$page.php";
    }
    elseif(file_exists("./includes/$page")) {
        include "./includes/$page";
    }else{
      echo "File is not exit ";
    }
}


function download($adfile, $file){
  //Only Administrators can download files .
      $cert = 'N';
    if(isset($adfile) && file_get_contents($adfile, 'r') === 'Yeah Everything Will Be Ok My Boss') {
      echo "Welcome ! You Are Administrator !";
      $cert = 'Y';
    }else{
      echo "error1";
    }
    if ($cert === 'Y'){
      if (stripos($file, 'file_list') != false) die('error4');
      if (stripos($file, 'file_list') >= 0) {
      header('Content-Description: File Transfer');
      header('Content-Type: application/octet-stream');
      header('Content-Disposition: attachment; filename='. basename($file));
      header('Content-Transfer-Encoding: binary');
      header('Expires: 0');
      header('Cache-Control: must-revalidate, post-check=0, pre-check=0');
      header('Pragma: public');
      header('Content-Length: ' . filesize($file));
      readfile($file);
    }else{
      die('error2');
    }
}else{
  echo 'error3';
}
}

if(!isset($_GET['page'])) {
    $page = 'index';
}
else {
    $page = $_GET['page'];
}
if (stripos($page, './') > 0) {
  die('no ./ ./ ./ ./');
}
if (stripos($page, '://') > 0) {
  die('no :// :// ://');
}
autoload($page);

if (isset($_GET[admin]) && isset($_GET[file])) {

  if (stripos($_GET[admin], 'flag') > 0 || stripos($_GET[file], 'flag') > 0) {
    die('not flag flag flag falg !');
  }

  if (strlen($_GET[file]) >= 38) {
    die('too long');
  }

  download($_GET[admin], $_GET[file]);
}


?>

```

# Refference
+ 赛博地球杯工业互联网安全大赛 工控云管理系统客服中心期待您的反馈