# Challenge
```php 
class TokenStorage {
    public function performAction($action, $data) {
        switch ($action) {
            case 'create':
                $this->createToken($data);
                break;
            case 'delete':
                $this->clearToken($data);
                break;
            default:
                throw new Exception('Unknown action');
        }
    }

    public function createToken($seed) {
        $token = md5($seed);
        file_put_contents('/tmp/tokens/' . $token, '...data');
    }

    public function clearToken($token) {
        $file = preg_replace("/[^a-z.-_]/", "", $token);
        unlink('/tmp/tokens/' . $file);
    }
}

$storage = new TokenStorage();
$storage->performAction($_GET['action'], $_GET['data']);
```

# Solution
This challenge contains a file delete vulnerability. The bug causing this issue is a non-escaped hyphen character (-) in the regular expression that is used in the preg_replace() call in line 21. If the hyphen is not escaped, it is used as a range indicator, leading to a replacement of any character that is not a-z or an ASCII character in the range between dot (46) and underscore (95). Thus dot and slash can be used for directory traversal and (almost) arbitrary files can be deleted, for example with the query parameters action=delete&data=../../config.php.

# Refference
+ php-security-calendar-2017 Day 6 - Frost Pattern