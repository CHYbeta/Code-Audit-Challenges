# Challenge
```php 
class Redirect {
    private $websiteHost = 'www.example.com';

    private function setHeaders($url) {
        $url = urldecode($url);
        header("Location: $url");
    }

    public function startRedirect($params) {
        $parts = explode('/', $_SERVER['PHP_SELF']);
        $baseFile = end($parts);
        $url = sprintf(
            "%s?%s",
            $baseFile,
            http_build_query($params)
        );
        $this->setHeaders($url);
    }
}

if ($_GET['redirect']) {
    (new Redirect())->startRedirect($_GET['params']);
}
```

# Solution
This challenge contains an open redirect vulnerability in line 6. The code takes the input from the super global $_SERVER[‘PHP_SELF’] and splits it at the slash character (line 10). Then the last part is taken and used to build the new URL that is passed into the header() function. An attacker is able to inject his own URL by using url-encoded characters that are decoded in line 5. A possible payload could look like /index.php/http:%252f%252fwww.domain.com?redirect=1.
This bug allows an attacker to redirect the user from the original site to a site of the attackers will. The attacker then could do phishing, for example he could present a forged login screen to grab the credentials of the user for the original site.

# Refference
+ php-security-calendar-2017 Day 15 - Sleigh Ride