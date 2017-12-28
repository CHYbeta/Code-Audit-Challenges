# Challenge
```php 
@$GLOBALS=$GLOBALS{next}=next($GLOBALS{'GLOBALS'})
[$GLOBALS['next']['next']=next($GLOBALS)['GLOBALS']]
[$next['GLOBALS']=next($GLOBALS[GLOBALS]['GLOBALS'])
[$next['next']]][$next['GLOBALS']=next($next['GLOBALS'])]
[$GLOBALS[next]['next']($GLOBALS['next']{'GLOBALS'})]=
next(neXt(${'next'}['next']));
```

# Solution
This challenge consists of a code snippet that was created by one of our team members for the Hack.lu CTF Tournament. It makes heavy use of the next() function and the $GLOBALS array. The next() function moves the internal array pointer up by one. Combined with the $GLOBALS array this allows us to execute arbitrary code.
The payload has to be split up into 2 segments: First, a PHP function to execute, passed in via $_COOKIE[‘GLOBALS’]. Second, parameters for the injected function, passed in via the file type of a sent file with the same name as the called PHP function. A more detailed write-up of the solution can be found here.

https://github.com/ctfs/write-ups-2014/tree/master/hack-lu-ctf-2014/next-global-backdoor

# Refference
+ php-security-calendar-2017 