# Challenge 
```php
<?php
  ($_=@$_GET['orange']) && @substr(file($_)[0],0,6) === '@<?php' ? include($_) : highlight_file(__FILE__);
```

# Refference
+ hitcon ctf 2018 one-line-php-challenge
+ https://github.com/orangetw/My-CTF-Web-Challenges/tree/master/hitcon-ctf-2018/one-line-php-challenge
