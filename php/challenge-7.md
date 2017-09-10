# Challenge  
```php 
<?php
$output = "";
if (isset($_GET['code'])) {
  $content = file_get_contents(__FILE__);
  $content = preg_replace('/FLAG\-[0-9a-zA-Z_?!.,]+/i', 'FLAG-XXXXXXXXXXXXXXXXXXXXXXX', $content);
  echo '<div class="code-highlight">';
  highlight_string($content);
  echo '</div>';
}
if (isset($_GET['pass'])) {
  if(!preg_match('/^[^\W_]+$/', $_GET['pass'])) {
    $output = "Don't hack me please :(";
  } else {
    $pass = md5("admin1674227342");
    if ((((((((($_GET['pass'] == $pass)))) && (((($pass !== $_GET['pass']))))) || ((((($pass == $_GET['pass'])))) && ((($_GET['pass'] !== $pass)))))))) { // Trolling u lisp masta
      if (strlen($pass) == strlen($_GET['pass'])) {
        $output = "<div class='alert alert-success'>FLAG-XXXXXXXXXXXXXXXXXXXXXXX</div>";
      } else {
        $output = "<div class='alert alert-danger'>Wrong password</div>";
      }
    } else {
      $output = "<div class='alert alert-danger'>Wrong password</div>";
    }
  }
}
?>
```
# Solution 

考察php弱类型。经过md5加密后生成以0e开头的字符串，而以0e开头的字符串用==比较时会被转换成0 == 0即成立。而!== 不仅比较值，而且还会比较类型。所以我们只要传入pass的值为一个0e开头的值，并且长度为32位（$pass长度为32位），比如说：0e509367213418206700842008763514。


# Refference 
+ [PHP Fairy](https://chybeta.github.io/2017/06/30/%C2%96ringzer0team-web-writeup/#PHP-Fairy)
