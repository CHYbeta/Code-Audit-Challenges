# Challenge
```php
<?php
    if(isset($_POST['login']))
     {
        if(isset($_POST['user']))
        {
            if(@strcmp($_POST['user'],$USER))//USER是被隐藏的复杂用户名
            {
                die('user错误！');
            }
        }
        if (isset($_POST['name']) && isset($_POST['password']))
        {
            if ($_POST['name'] == $_POST['password'] )
            {
                die('账号密码不能一致！');
            }
            if (md5($_POST['name']) === md5($_POST['password']))
            {
                if(is_numeric($_POST['id'])&&$_POST['id']!=='72' && !preg_match('/\s/', $_POST['id']))
                {
                        if($_POST['id']==72)
                            die("flag{xxxxxxxxxxxxx}");
                        else
                            die("ID错误2！");
                }
                else
                {
                    die("ID错误1！");
                }
            }
            else
                die('账号密码错误！');
        }
     }
 ?>
```
# Solution 

# Refference
+ [CCTF-2017-WEB350](http://kafkatao.me/2017/07/23/4.html)