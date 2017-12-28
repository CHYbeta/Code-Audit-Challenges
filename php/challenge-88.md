# Challenge
```php 
class LDAPAuthenticator {
    public $conn;
    public $host;

    function __construct($host = "localhost") {
        $this->host = $host;
    }

    function authenticate($user, $pass) {
        $result = [];
        $this->conn = ldap_connect($this->host);    
        ldap_set_option(
            $this->conn,
            LDAP_OPT_PROTOCOL_VERSION,
            3
        );
        if (!@ldap_bind($this->conn))
            return -1;
        $user = ldap_escape($user, null, LDAP_ESCAPE_DN);
        $pass = ldap_escape($pass, null, LDAP_ESCAPE_DN);
        $result = ldap_search(
            $this->conn,
            "",
            "(&(uid=$user)(userPassword=$pass))"
        );
        $result = ldap_get_entries($this->conn, $result);
        return ($result["count"] > 0 ? 1 : 0);
    }
}

if(isset($_GET["u"]) && isset($_GET["p"])) {
    $ldap = new LDAPAuthenticator();
    if ($ldap->authenticate($_GET["u"], $_GET["p"])) {
        echo "You are now logged in!";
    } else {
        echo "Username or password unknown!";
    }
}
```

# Solution
The LDAPAuthenticator class is prone to an LDAP injection in line 24. By injecting special characters into the username it is possible to alternate the result set of the LDAP query. Although the ldap_escape() function is used to sanitize the input in lines 19 and 20, a wrong flag has been passed to the sanitize-calls resulting in insufficient/incorrect sanitization. Therefore, in this particular example, the LDAP injection results in an unauthenticated adversary bypassing the authentication mechanism by injecting the asterisk-wildcard * character as username and password to successfully login as an arbitrary user.

# Refference
+ php-security-calendar-2017 Day 23 - Cookies