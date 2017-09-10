# Challenge 
```php 
<?php
    #made by adm1nkyj
    error_reporting(0);
    include('./flag.php');
    
    echo 'login as admin kkk<br/>';

    $filter = ['conv', 'code', 'hex', 'ha', 'b', 'x', '_', '`', '\'', '"', '@','into','outfile','load','file', 'date', 'co','ca', 'b', 'g', 'h', 'j', 'k', 'q', 'v', 'x', 'z', 'date', 'make', 'day', 'name', 'replace', 'insert', 'pad', 'ascii', 'user', 'version', 'db', 'data', 'base'];

    $id = addslashes($_GET['id']);
    $pw = addslashes($_GET['pw']);
    $id = mb_convert_encoding($id, 'utf-8', 'euc-kr');
    if(strlen($pw)>=370) die('no hack');
    foreach ($filter as $_str) 
    { 
        if(strpos(strtolower($_GET['id']), $_str) !== false || strpos(strtolower($_GET['pw']), $_str) !== false)
        {
            echo $_str;
            exit('no hack');
        }
    }
    if(preg_match('/[0-9]/', $_GET['id']) || preg_match('/[0-9]/', $_GET['pw']))
    {
        exit('no hack');
    }
    
    $query = mysql_fetch_array(mysql_query("SELECT * FROM user WHERE id='{$id}' AND pw='{$pw}';"));
    if($query['id'])
    {
        if(strtolower($query['id']) === 'admin')
        {
            exit($flag);
        }
        else
        {
            echo "your id : ".$query['id'];
        }
    }
    echo "<hr>";
    show_source(__FILE__);
?>
```

# Solution

# Refference
+ [Secuinside CTF 2017 Web415: Mathboy7 Writeup](http://blog.samueltang.net/archives/68)