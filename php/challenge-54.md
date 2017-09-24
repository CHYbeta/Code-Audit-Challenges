# Challenge
```php 
<!DOCTYPE html>
<html>
   <title>神秘的验证码</title>
   </head>
   <body>
<?php
define("key", "*********************");
define("method", "aes-128-cbc");
function get_token()
{
    $token = '';
    for($i = 0 ; $i < 16 ; $i++ )
    {
        $token.=chr(rand(1,255));
    }
    return $token;
}
function enc($s)
{   
    $token = get_token();
    $cc = openssl_encrypt((string)$s, method, key, OPENSSL_RAW_DATA, $token);
    $cccc = base64_encode(base64_encode($token.'_'.$cc));
    return $cccc;   
}
function dec($s)
{
    if($cc = base64_decode(base64_decode($s)))
    {
        if($iv = substr($cc,0,16))
        {
            if($d = substr($cc,17))
            {
                if($s = openssl_decrypt($d, method, key, OPENSSL_RAW_DATA,$iv))
                {
                	return $s;
                }
                else
                    die("error");
            }
            else
                return 0;
        }
        else
            return 0;
    }
    else
        return 0;
}
echo "<p hidden>".enc(1)."</p>\n</br>\n";
?>
<?php 
session_start();
$str = '';
$s1 = mt_rand(1, 9);
$s2 = mt_rand(1, 9);
$s3 = mt_rand(1, 9);
$s4 = mt_rand(1, 9);
$value = $s1.$s2.$s3.$s4;
$str.='<span style="color:rgb('.mt_rand(0, 255).','.mt_rand(0, 255).','.mt_rand(0, 255).')">'.$s1.'<span>';
$str.='<span style="color:rgb('.mt_rand(0, 255).','.mt_rand(0, 255).','.mt_rand(0, 255).')">'.$s2.'<span>';
$str.='<span style="color:rgb('.mt_rand(0, 255).','.mt_rand(0, 255).','.mt_rand(0, 255).')">'.$s3.'<span>';
$str.='<span style="color:rgb('.mt_rand(0, 255).','.mt_rand(0, 255).','.mt_rand(0, 255).')">'.$s4.'<span>';
?>

<form action="index.php" method="post">
	<label for="check">请输入验证码:</label>
	<p><input type="text" name="check_code"><?php echo $str;?></p>
	<p><input type="submit" name="submit" value="Submit"></p>
</form>

<?php
if(isset($_POST['check_code'])){
	if($_POST['check_code'] === $_SESSION['value']){
		if(dec($_GET['secret']) === md5($_SESSION['value'])){
			echo "WDFLAG{**********}";
		}
		else{
			die("Code true but secret is error.");
		}
	}
	else{
		echo "Code error!";
	}
}
$_SESSION['value'] = $value;
 ?>
<!--Firstly, You need to get secret!-->
</body>
</html>
```

# Refference 
+ 2017 问鼎杯 决赛