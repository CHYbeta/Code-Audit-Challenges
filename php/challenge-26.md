# Challenge
```php 
<?php
$flag = "xxx";
if (isset($_POST['answer'])){
		$number = $_POST['answer'];
        if (noother_says_correct($number)){
                echo $flag;
        }  else {
                echo "Sorry";
        }
}

function noother_says_correct($number)
{
        $one = ord('1');
        $nine = ord('9');
        # Check all the input characters!        
        for ($i = 0; $i < strlen($number); $i++)
        { 
                # Disallow all the digits!
                $digit = ord($number{$i});
                if ( ($digit >= $one) && ($digit <= $nine) )                
                {
                        # Aha, digit not allowed!
                        return false;
                }
        }        
        # Allow the magic number ...
        return $number == "3735929054";
}
?>
```

# Solution
题目要求我们传入的每一位不允许是1到9的数字。而。
```python 
>>> hex(3735929054)
'0xdeadc0de'
```
恰好3735929054的十六进制为0xdeadc0de，仅出现字母与数字0，因此可以绕过检测。

最后与3735929054进行`==`比较，这里存在php弱类型比较问题，即"0xdeadc0de" == "3735929054"。

payload：
```
POST: answer=0xdeadc0de
```

# Refference 
+ [PHP 0818](https://www.wechall.net/challenge/noother/php0818/index.php?highlight=christmas)
