# Challenge
```php
$url = 'file://localhost/etc/passwd'
$parts = parse_url($url);
if (empty($parts['hosts']) || $parts['host'] != 'localhost'){
    exit('error');
}
readfile($url);
?>
```
# Solution 

# Refference