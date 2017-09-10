# Challenge 
```php 
<html>
<body>
<?php
include 'hanshu.php';
if(isset($_GET['do']))
{
    $do=$_GET['do'];
    if($do==upload)
    {
        if(empty($_FILES))
        {
            $html1=<<<HTML1
            <form action="index.php?do=upload" method="post" enctype="multipart/form-data">
            <input type="file" name="filename">                 
            <input type="submit" value="upload">
            </form>
HTML1;
            echo $html1;
        }
        else
        {   $file=@file_get_contents($_FILES["filename"]["tmp_name"]);
            if(empty($file))
            {
                die('do you upload a file?');
            }
            else
            {
                if((strpos($file,'<?')>-1)||(strpos($file,'?>')>-1)||(stripos($file,'php')>-1)||(stripos($file,'<script')>-1)||(stripos($file,'</script')>-1))
                {
                    die('you can\' upload this!');
                }
                else
                {
                    $rand=mt_rand();
                    $path='/var/www/html/web-03/uploads/'.$rand.'.txt';
                    file_put_contents($path, $file);
                    echo 'your upload success!./uploads/'.$rand.'.txt';
                }
            }
            
        }
        
    }
    elseif($do==rename)
    {
        if(isset($_GET['re']))
        {
            $re=$_GET['re'];
            $re2=@unserialize(base64_decode(unKaIsA($re,6)));
            if(is_array($re2))
            {
                if(count($re2)==2)
                {   
                    $rename='txt';
                    $rand=mt_rand();
                    $fp=fopen('./uploads/'.$rand.'.txt','w');
                    foreach($re2 as $key=>$value)
                    {
                        if($key==0)
                        {
                            $rename=$value;
                        }
                        else
                        {
                            if(file_exists('./uploads/'.$value.'.txt')&&is_numeric($value))
                            {
                                $file=file_get_contents('./uploads/'.$value.'.txt');
                                fwrite($fp,$file);
                            }
                        }
                    }
                    fclose($fp);
                    waf($rand,$rename);
                    rename('./uploads/'.$rand.'.txt','./uploads/'.$rand.'.'.$rename);
                    echo "you success rename!./uploads/$rand.$rename";
                }
            }
            else
            {
                echo 'please not hack me!';
            }
        }
        elseif(isset($_POST['filetype'])&&isset($_POST['filename']))
        {
            $filetype=$_POST['filetype'];
            $filename=$_POST['filename'];
            if((($filetype=='jpg')||($filetype=='png')||($filetype=='gif'))&&is_numeric($filename))
            {   
                $re=KaIsA(base64_encode(serialize(array($filetype,$filename))),6);
                header("Location:index.php?do=rename&re=$re");
                exit();
            }
            else
            {
                echo 'you do something wrong';
            }
        }
        else
        {
            $html2=<<<HTML2
            <form action="index.php?do=rename" method="post">          
filetype: <input type="text" name="filetype" /> please input the your file's type
</br>
filename: <input type="text" name="filename" /> please input your file's numeric name,like 12345678
</br>
<input type="submit" />
</form>
HTML2;
            echo $html2;
            
        }
    }
    
}
else
{   
    show_source(__FILE__);
}
?>
</body>
</html>
```

# Solution 

# Refference
+ [ISCC2017Web : I have a jpg,i upload a txt.](http://pupiles.com/ISCC.html)