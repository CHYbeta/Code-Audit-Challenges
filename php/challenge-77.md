# Challenge
```php 
class LoginManager {
    private $em;
    private $user;
    private $password;

    public function __construct($user, $password) {
        $this->em = DoctrineManager::getEntityManager();
        $this->user = $user;
        $this->password = $password;
    }

    public function isValid() {
        $user = $this->sanitizeInput($this->user);
        $pass = $this->sanitizeInput($this->password);
        
        $queryBuilder = $this->em->createQueryBuilder()
            ->select("COUNT(p)")
            ->from("User", "u")
            ->where("user = '$user' AND password = '$pass'");
        $query = $queryBuilder->getQuery();
        return boolval($query->getSingleScalarResult());
    }
	
    public function sanitizeInput($input, $length = 20) {
        $input = addslashes($input);
        if (strlen($input) > $length) {
            $input = substr($input, 0, $length);
        }
        return $input;
    }
}

$auth = new LoginManager($_POST['user'], $_POST['passwd']);
if (!$auth->isValid()) {
    exit;
}
```

# Solution
Today's challenge contains a DQL (Doctrine Query Language) injection vulnerability in line 19. A DQL injection is similar to a SQL injection but more limited, nonetheless the where() method of Doctrine is vulnerable. In line 13 and 14 sanitization is added to the input, however, the sanitizeInput() method has a bug. First, it uses addslashes() for escaping relevant characters by adding a backslash \ infront of them. In this case if we pass a \ as input, it get escaped to \\. But then, the substr() function is used to truncate the escaped string. This enables an attacker to send a string that is long enough that the escaped backslash is cut off and we are left with a single \ at the end of the string. This will then break the WHERE statement and allows the injection of own DQL syntax, for example the condition OR 1=1 that is always true and bypasses the authentication: user=1234567890123456789\&passwd=%20OR%201=1-. The resulting WHERE statement will look like user = '1234567890123456789\' AND password = ' OR 1=1-' in DQL. Note how the backslash confuses the quotes and allows to inject DQL into the password value. The resulting query does not look valid because of the trailing slash. Fortunately, Doctrine closes the last single quote on its own, so the resulting query looks like OR 1=1-''.
To avoid DQL injections always use bound parameters for dynamic conditions. Never try to secure a DQL query with addslashes() or similar functions. Additionally, the password should be stored hashed in the database, for example in the BCrypt format.

# Refference
+ php-security-calendar-2017 Day 13 - Turkey Baster