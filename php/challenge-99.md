# Challenge
```php
<?php 

$SECRET  = `../read_secret`;                                  
$SANDBOX = "../data/" . md5($SECRET. $_SERVER["REMOTE_ADDR"]); 
$FILEBOX = "../file/" . md5("K0rz3n". $_SERVER["REMOTE_ADDR"]);    
@mkdir($SANDBOX); 
@mkdir($FILEBOX); 



if (!isset($_COOKIE["session-data"])) { 
    $data = serialize(new User($SANDBOX)); 
    $hmac = hash_hmac("md5", $data, $SECRET); 
    setcookie("session-data", sprintf("%s-----%s", $data, $hmac));       
} 


class User { 
    public $avatar; 
    function __construct($path) { 
        $this->avatar = $path;                                          
    } 
} 


class K0rz3n_secret_flag { 
    protected $file_path; 
    function __destruct(){ 
        if(preg_match('/(log|etc|session|proc|read_secret|history|class)/i', $this->file_path)){ 
            die("Sorry Sorry Sorry"); 
        } 
    include_once($this->file_path); 
 } 
} 


function check_session() { 
    global $SECRET; 
    $data = $_COOKIE["session-data"]; 
    list($data, $hmac) = explode("-----", $data, 2); 
    if (!isset($data, $hmac) || !is_string($data) || !is_string($hmac)){ 
        die("Bye"); 
    } 
    if ( !hash_equals(hash_hmac("md5", $data, $SECRET), $hmac) ){ 
        die("Bye Bye"); 
    } 
    $data = unserialize($data); 

    if ( !isset($data->avatar) ){ 
        die("Bye Bye Bye"); 
    } 
    return $data->avatar;                                               
} 


function upload($path) { 
    if(isset($_GET['url'])){ 
         if(preg_match('/^(http|https).*/i', $_GET['url'])){ 
            $data = file_get_contents($_GET["url"] . "/avatar.gif");                                                                                     
            if (substr($data, 0, 6) !== "GIF89a"){ 
                die("Fuck off"); 
            } 
            file_put_contents($path . "/avatar.gif", $data); 
            die("Upload OK"); 
        }else{ 
            die("Hacker"); 
        }            
    }else{ 
        die("Miss the URL~~"); 
    } 
} 


function show($path) { 
    if ( !is_dir($path) || !file_exists($path . "/avatar.gif")) { 
            
        $path = "/var/www"; 
    } 
    header("Content-Type: image/gif"); 
    die(file_get_contents($path . "/avatar.gif"));                     
} 


function check($path){ 
    if(isset($_GET['c'])){ 
        if(preg_match('/^(ftp|php|zlib|data|glob|phar|ssh2|rar|ogg|expect)(.|\\s)*|(.|\\s)*(file)(.|\\s)*/i',$_GET['c'])){ 
            die("Hacker Hacker Hacker"); 
        }else{ 
            $file_path = $_GET['c']; 
            list($width, $height, $type) = @getimagesize($file_path); 
            die("Width is ：" . $width." px<br>" . 
                "Height is ：" . $height." px<br>"); 
        } 
    }else{ 
        list($width, $height, $type) = @getimagesize($path."/avatar.gif"); 
        die("Width is ：" . $width." px<br>" . 
            "Height is ：" . $height." px<br>"); 
    } 
} 


function move($source_path,$dest_name){ 
    global $FILEBOX; 
    $dest_path = $FILEBOX . "/" . $dest_name; 
    if(preg_match('/(log|etc|session|proc|root|secret|www|history|file|\.\.|ftp|php|phar|zlib|data|glob|ssh2|rar|ogg|expect|http|https)/i',$source_path)){ 
        die("Hacker Hacker Hacker"); 
    }else{ 
        if(copy($source_path,$dest_path)){ 
            die("Successful copy"); 
        }else{ 
            die("Copy failed"); 
        } 
    } 
} 




$mode = $_GET["m"]; 

if ($mode == "upload"){ 
     upload(check_session()); 
} 
else if ($mode == "show"){ 
    show(check_session()); 
} 
else if ($mode == "check"){ 
    check(check_session()); 
} 
else if($mode == "move"){ 
    move($_GET['source'],$_GET['dest']); 
} 
else{ 
     
    highlight_file(__FILE__);     
} 

include("./comments.html"); 

```
# Refference
+ Lctf 2018 T4lk 1s ch34p,sh0w m3 the sh31l