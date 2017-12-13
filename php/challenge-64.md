# Challenge 
```php 
<?php
$fail = str_repeat('fail', 100);
$d = 'sandbox/FAIL_' . sha1($_SERVER['REMOTE_ADDR'] . '95aca804b832f4c329d8c0e7c789b02b') . '/';
@mkdir($d);

function read_ok($f)
{
    return strstr($f, 'FAIL_') === FALSE &&
        strstr($f, '/proc/') === FALSE &&
        strstr($f, '/dev/') === FALSE;
}

function write_ok($f)
{
    return strstr($f, '..') === FALSE && read_ok($f);
}

function GetDirectorySize($path)
{
    $bytestotal = 0;
    $path = realpath($path);
    if ($path !== false && $path != '' && file_exists($path)) {
        foreach (new RecursiveIteratorIterator(new RecursiveDirectoryIterator($path, FilesystemIterator::SKIP_DOTS)) as $object) {
            $bytestotal += $object->getSize();
        }
    }
    return $bytestotal;
}

if (isset($_GET['action'])) {
    if ($_GET['action'] == 'pwd') {
        echo $d;

        exit;
    }
    else if ($_GET['action'] == 'phpinfo') {
        phpinfo();

        exit;
    }
    else if ($_GET['action'] == 'read') {
        $f = $_GET['filename'];
        if (read_ok($f))
            echo file_get_contents($d . $f);
        else
            echo $fail;

        exit;
    } else if ($_GET['action'] == 'write') {
        $f = $_GET['filename'];
        if (write_ok($f) && strstr($f, 'ph') === FALSE && $_FILES['file']['size'] < 10000) {
            print_r($_FILES['file']);
            print_r(move_uploaded_file($_FILES['file']['tmp_name'], $d . $f));
        }
        else
            echo $fail;

        if (GetDirectorySize($d) > 10000) {
            rmdir($d);
        }

        exit;
    } else if ($_GET['action'] == 'delete') {
        $f = $_GET['filename'];
        if (write_ok($f))
            print_r(unlink($d . $f));
        else
            echo $fail;

        exit;
    }
}

highlight_file(__FILE__);
```

# Refference
+ SECCON 2017 QUAL WEB automatic_door