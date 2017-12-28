# Challenge 
```
class Login {
    public function __construct($user, $pass) {
        $this->loginViaXml($user, $pass);
    }

    public function loginViaXml($user, $pass) {
        if (
            (!strpos($user, '<') || !strpos($user, '>')) &&
            (!strpos($pass, '<') || !strpos($pass, '>'))
        ) {
            $format = '<?xml version="1.0"?>' .
                      '<user v="%s"/><pass v="%s"/>';
            $xml = sprintf($format, $user, $pass);
            $xmlElement = new SimpleXMLElement($xml);
            // Perform the actual login.
            $this->login($xmlElement);
        }
    }
}

new Login($_POST['username'], $_POST['password']);
```

# Solution
This challenge suffers from an XML injection vulnerability in line 14. An attacker can manipulate the XML structure and hence bypass the authentication. There is an attempt to prevent exploitation in lines 8 and 9 by searching for angle brackets but the check can be bypassed with a specifically crafted payload. The bug in this code is the automatic casting of variables in PHP. The PHP built-in function strpos() returns the numeric position of the looked up character. This can be 0 if the first character is the one searched for. The 0 is then type-casted to a boolean false for the if comparison which renders the overall constraint to true. A possible payload could look like user=<"><injected-tag%20property="&pass=<injected-tag>.

# Refference
+ php-security-calendar-2017 