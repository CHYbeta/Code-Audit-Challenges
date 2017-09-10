# Challege 
```php 
<?php 
error_reporting(0);
function RotEncrypt($str, $pass){
   $pass = str_split(str_pad('', strlen($str), $pass, STR_PAD_RIGHT));
   $stra = str_split($str);
   foreach($stra as $k=>$v){
     $tmp = ord($v)+ord($pass[$k]);
     $stra[$k] = chr( $tmp > 255 ?($tmp-256):$tmp);
   }
   return join('', $stra);
}
function post($url, $post_data = '', $timeout = 5){
    $ch = curl_init();
    curl_setopt ($ch, CURLOPT_URL, $url);
        curl_setopt ($ch, CURLOPT_POST, 1);
    if($post_data != ''){
        curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
    }
    curl_setopt ($ch, CURLOPT_RETURNTRANSFER, 1); 
    curl_setopt ($ch, CURLOPT_CONNECTTIMEOUT, $timeout);
    curl_setopt($ch, CURLOPT_HEADER, false);
    $file_contents = curl_exec($ch);
    curl_close($ch);
    return $file_contents;
}
$name = addslashes($_POST['name']);
$cat = addslashes($_POST['cat']);
$content = <<< EOF
    <div style="text-align:center;margin-top:150px;">
    <h3>Book search system</h3>
    <form action="admin.php" method="post">
        Name: <input type="text" name="name" value="king"></input><br>
        Category: &nbsp;<select name="cat">
        <option value ="Classic Literature & Fiction">Classic Literature & Fiction</option>
        <option value ="Literary">Literary</option>
        <option value ="Literature & Fiction">Literature & Fiction</option>
        <option value ="Military History">Military History</option>
        <option value ="Thrillers & Suspense">Thrillers & Suspense</option>
        <option value ="Historical">Historical</option>
        </select>
        <input type="submit" name="submit" value="Query"></input><br>
    </form>
    </div>
EOF;
echo $content;
if($name && $cat){
    echo post("http://10.18.25.154:10002/isc/query.php",array("data"=>RotEncrypt("name=$name&cat=$cat","ISC2015")));
}
if($_POST['key'] == "{$key}"){
    system($_GET['cmd']);
}
?>
/*
query.php:
include "config.php";
function RotDecrypt($str, $pass){
   $pass = str_split(str_pad('', strlen($str), $pass, STR_PAD_RIGHT));
   $stra = str_split($str);
   foreach($stra as $k=>$v){
     $tmp = ord($v)-ord($pass[$k]);
     $stra[$k] = chr( $tmp < 0 ?($tmp+256):$tmp);
   }
   return join('', $stra);
}
function Fsql($sql){
    if(preg_match('/(and|ascii|concat|from|group by|group_concat|hex|limit|lpad|or|select|substr|union|where|\s)/i', $sql)){
        return "";
    }else{
        return $sql;
    }
}
parse_str(RotDecrypt($_POST['data'],"ISC2015"), $str);
$connection = mysql_connect($db_host,$db_username,$db_password) or die("could not connect to Mysql");
mysql_query("set names 'utf8'");
$db_selecct=mysql_select_db($db_database) or die("could not to the database");
$query="select * from test where name = '".Fsql($str[name])."'";
$result = @mysql_query($query);
if($result){
    $res=mysql_fetch_array($result);
    if($res['name']){
        echo $str[name]." exist.";
    }else{
        echo $str[name]." not exist.";
    }
}
*/
```


# Refference
+ [ISC2015攻防挑战赛靶机攻略:PHP代码审计类](http://www.moonsos.com/post/147.html)