# Challenge
```php
function getUser($id) {
    global $config, $db;
    if (!is_resource($db)) {
        $db = new MySQLi(
            $config['dbhost'],
            $config['dbuser'],
            $config['dbpass'],
            $config['dbname']
        );
    }
    $sql = "SELECT username FROM users WHERE id = ?";
    $stmt = $db->prepare($sql);
    $stmt->bind_param('i', $id);
    $stmt->bind_result($name);
    $stmt->execute();
    $stmt->fetch();
    return $name;
}

$var = parse_url($_SERVER['HTTP_REFERER']);
parse_str($var['query']);
$currentUser = getUser($id);
echo '<h1>'.htmlspecialchars($currentUser).'</h1>';
 
```

# Solution
This challenge suffers from a connection string injection vulnerability in line 4. It occurs because of the parse_str() call in line 21 that behaves very similar to register globals. Query parameters from the referrer are extracted to variables in the current scope, thus we can control the global variable $config inside of getUser() in lines 5 to 8. To exploit this vulnerability we can connect to our own MySQL server and return arbitrary values for username, for example with the referrer http://host/?config[dbhost]=10.0.0.5&config[dbuser]=root&config[dbpass]=root&config[dbname]=malicious&id=1.

# Refference
+ php-security-calendar-2017 Day 7 - Bells