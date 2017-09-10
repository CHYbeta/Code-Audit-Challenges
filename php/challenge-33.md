# Challenge 
```php 
<?php
/**
 * Created by PhpStorm.
 * User: phithon
 * Date: 16/6/8
 * Time: 上午12:24
 */ 

//控制报错显示源码
error_reporting(-1);
ini_set("display_errors", 1);
if(isset($_GET['x_show_source'])) {
    show_source(__FILE__);
    exit;
}   

//为每次会话开启session
session_start();

//根据rand_str()生成6位SECRET_KEY和16位CSRF_TOKEN
if(empty($_SESSION['SECRET_KEY'])) {
    $_SESSION['SECRET_KEY'] = rand_str(6);
}
if(empty($_SESSION['CSRF_TOKEN'])) {
    $_SESSION['CSRF_TOKEN'] = rand_str(16);
}   

//包含点，其中可能存在flag
include_once "flag.php";    

//使用rand()函数随机生成指定长度字符串
function rand_str($length = 16)
{
    $rand = [];
    $_str = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    for($i = 0; $i < $length; $i++) {
        $n = rand(0, strlen($_str) - 1);
        $rand[] = $_str{$n};
    }
    return implode($rand);
}   

//对ajax的请求以json形式相应，否则直接转换成字符串输出
function output($obj)
{
    if(isset($_SERVER['HTTP_X_REQUESTED_WITH']) &&
        strcasecmp($_SERVER['HTTP_X_REQUESTED_WITH'], 'XMLHttpRequest') === 0) {
        header("Content-Type: application/json");
        echo json_encode($obj);
    } else {
        header("Content-Type: text/html; charset=UTF-8");
        echo strval($obj);
    }
}   

//每次提交check之后，将CSRF_TOKEN置为null
function check_csrf_token()
{
    if(empty($_SESSION['CSRF_TOKEN']) || $_POST['CSRF_TOKEN'] !== $_SESSION['CSRF_TOKEN']) {
        return false;
    } else {
        $_SESSION['CSRF_TOKEN'] = null;
        return true;
    }
}   

//显示form页面
function show_form_page()
{
    ?>
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>safebox</title>
        <link rel="stylesheet" href="style.css">
    </head>
    <body>  

    <div class="container">
        <form method="post">
        <div class="block title">
            安全箱子
        </div>
        <div class="block show">
            <div class="line">
                <label>输入验证字符串: </label>
                <input type="text" name="key">
            </div>
            <div class="line">
                <label>输入方法　　　: </label>
                <input type="text" name="act">
            </div>
        </div>
        <div class="block info">
            <input type="reset" value="重置">
            <input name="submit" type="submit" value="提交">
            <input type="hidden" name="CSRF_TOKEN" value="<?php echo $_SESSION['CSRF_TOKEN'] ?>">
        </div>
        </form>
    </div>  

    </body>
    </html>
    <?php
}   

//显示报错页面
function show_error_page($msg)
{
    ?>
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Error</title>
        <link rel="stylesheet" href="style.css">
        <!-- ?x_show_source -->
    </head>
    <body>  

    <div class="container">
        <div class="block title">
            Error
        </div>
        <div class="block show">
            <?php echo $msg; ?>
        </div>
        <div class="block info">
            <a href="javascript:history.back(-1)">返回</a>
        </div>
    </div>  

    </body>
    </html>
    <?php
    exit;
}   

$act = isset($_POST['act']) ? $_POST['act'] : "";
$key = isset($_POST['key']) ? $_POST['key'] : "";
if(isset($_POST['submit']) && check_csrf_token()) {                 //csrf_token校验
    if(hash_hmac('md5', $act, $_SESSION['SECRET_KEY']) === $key) {  //hmac_md5校验
        if(function_exists($act)) {                                 //函数存在性校验
            $exec_res = $act();                                     //调用指定函数
            output($exec_res);                                      //输出函数返回结果
        } else {
            show_error_page("Function not found!!");
        }
    } else {
        show_error_page("Permission deny!!");
    }
} else {
    show_form_page();
}
```

# Solution 

# Refference
+ [不插电 · WooYun Puzzle#3 Write up](http://larry.ngrep.me/wooyun-pluzze-3-write-up.html)