# Challenge 
```php 
<?php
// closure, because of namespace!
$challenge = function()
{
        $f = Common::getGetString('eval');        $f = str_replace(array('`', '$', '*', '#', ':', '\\', '"', "'", '(', ')', '.', '>'), '', $f);
 
        if((strlen($f) > 13) || (false !== stripos($f, 'return')))
        {
                die('sorry, not allowed!');        }
 
        try
        {
                eval("\$spaceone = $f");        }
        catch (Exception $e)
        {
                return false;
        } 
        return ($spaceone === '1337');
};
?>
```

# Solution 
分析一下流程，通过get的eval参数传入并赋值到变量`$f`，然后经过str_replace()的过滤，要求长度小于13，并且不包含字符串return，接着执行eval。

目标是让结果返回True。最后一行`$spaceone === '1337'`，不存在弱类型比较，要求类型和值都得相等。看一下str_replace()，过滤了单引号，双引号，所以如果直接get传参`index.php?eval='1337'`进去，在经过过滤后，到最后会变为`$spaceone=1337`，是一个数值类型而非字符串。

查一下php手册；http://php.net/manual/zh/language.types.string.php 。除了用单引号，双引号表示字符串外，还有以下两种：
+ heredoc 语法结构
+ nowdoc 语法结构

用一个简单的例子，对heredoc语法结构如下：
```
$f = <<<q
1337
q;

```
`<<<`后面要提供一个标识符，这里为`q`，然后换行。接下来是字符串本身，这里为`1337`。结束时所引用的标识符必须在该行的第一列，即标识符`q`要在开头。标识符的命名只能包含字母、数字和下划线，并且必须以字母和下划线作为开头。在结束标识符这行除了可能有一个分号（;）外，绝对不能包含其它字符，更重要的是结束标识符的前面必须是个被本地操作系统认可的换行，比如在 UNIX 和 Mac OS X 系统中是 \n，而结束定界符（可能其后有个分号）之后也必须紧跟一个换行。

所以在本题中，我们构造payload如下：
```
https://www.wechall.net/challenge/space/php0819/index.php?eval=<<<q%0a1337%0aq;%0a
```
注意 %0a 即换行。在分号的最后还有一个%0a

而nowdoc语法结构中，由于`<<<`后的标识符要用单引号括起来，所以这里无法利用，不展开。

# Refference 
+ [WeChall PHP 0819](https://www.wechall.net/challenge/space/php0819/index.php)