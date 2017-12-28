# Challenge
```php 
class FTP {
    public $sock;

    public function __construct($host, $port, $user, $pass) {
        $this->sock = fsockopen($host, $port);

        $this->login($user, $pass);
        $this->cleanInput();
        $this->mode($_REQUEST['mode']);
        $this->send($_FILES['file']);
    }

    private function cleanInput() {
        $_GET = array_map('intval', $_GET);
        $_POST = array_map('intval', $_POST);
        $_COOKIE = array_map('intval', $_COOKIE);
    }

    public function login($username, $password) {
        fwrite($this->sock, "USER " . $username . "\n");
        fwrite($this->sock, "PASS " . $password . "\n");
    }

    public function mode($mode) {
        if ($mode == 1 || $mode == 2 || $mode == 3) {
            fputs($this->sock, "MODE $mode\n");
        }
    }

    public function send($data) {
        fputs($this->sock, $data);
    }
}
```

# Solution
This challenge contains two bugs that can be used together to inject data into the open FTP connection. The first bug is the usage of $_REQUEST in line 9 while only sanitizing $_GET and $_POST in lines 14 to 16. $_REQUEST is the combination of $_GET, $_POST, and $_COOKIE but it is only a copy of the values, not a reference. Therefore the sanitization of $_GET, $_POST, and $_COOKIE alone is not sufficient. A real world example of a vulnerability that is caused by a similar confusion can be found in our blog.

The second bug is the usage of the type-unsafe comparison == instead of === in line 25. This enables an attacker to inject and execute new commands in the existing connection, for example a delete command with the query string ?mode=1%0a%0dDELETE%20test.file.

# Refference
+ php-security-calendar-2017 Day 16 - Poem