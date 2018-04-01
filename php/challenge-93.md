# Challenge 
```php 
 <?php

error_reporting(0);

$dir = 'sandbox/' . sha1($_SERVER['REMOTE_ADDR']) . '/';
if(!file_exists($dir)){
  mkdir($dir);
}
if(!file_exists($dir . "index.php")){
  touch($dir . "index.php");
}

function clear($dir)
{
  if(!is_dir($dir)){
    unlink($dir);
    return;
  }
  foreach (scandir($dir) as $file) {
    if (in_array($file, [".", ".."])) {
      continue;
    }
    unlink($dir . $file);
  }
  rmdir($dir);
}

switch ($_GET["action"] ?? "") {
  case 'pwd':
    echo $dir;
    break;
  case 'phpinfo':
    echo file_get_contents("phpinfo.txt");
    break;
  case 'reset':
    clear($dir);
    break;
  case 'time':
    echo time();
    break;
  case 'upload':
    if (!isset($_GET["name"]) || !isset($_FILES['file'])) {
      break;
    }

    if ($_FILES['file']['size'] > 100000) {
      clear($dir);
      break;
    }

    $name = $dir . $_GET["name"];
    if (preg_match("/[^a-zA-Z0-9.\/]/", $name) ||
      stristr(pathinfo($name)["extension"], "h")) {
      break;
    }
    move_uploaded_file($_FILES['file']['tmp_name'], $name);
    $size = 0;
    foreach (scandir($dir) as $file) {
      if (in_array($file, [".", ".."])) {
        continue;
      }
      $size += filesize($dir . $file);
    }
    if ($size > 100000) {
      clear($dir);
    }
    break;
  case 'shell':
    ini_set("open_basedir", "/var/www/html/$dir:/var/www/html/flag");
    include $dir . "index.php";
    break;
  default:
    highlight_file(__FILE__);
    break;
}

```

# Refference
+ 0ctf 2018 ezdoor