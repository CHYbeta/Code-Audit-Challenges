# Challenge 
```php 
<?php 
function curl($url){  
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    $re = curl_exec($ch);
    curl_close($ch);
    return $re;
}
$url = $_GET['url'];
echo curl($url);
?>
```

# Refference
+ [Curl 导致 SSRF 及 WAF 绕过方式](http://sec2hack.com/web/curl-ssrf-waf.html)