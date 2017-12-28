# Challenge
```php 
declare(strict_types=1);

class ParamExtractor {
    private $validIndices = [];

    private function indices($input) {
        $validate = function (int $value, $key) {
            if ($value > 0) {
                $this->validIndices[] = $key;
            }
        };

        try {
            array_walk($input, $validate, 0);
        } catch (TypeError $error) {
            echo "Only numbers are allowed as input";
        }

        return $this->validIndices;
    }

    public function getCommand($parameters) {
        $indices = $this->indices($parameters);
        $params = [];
        foreach ($indices as $index) {
            $params[] = $parameters[$index];
        }
        return implode($params, ' ');
    }
}

$cmd = (new ParamExtractor())->getCommand($_GET['p']);
system('resizeImg image.png ' . $cmd);
```

# Solution
This challenge contains a command injection vulnerability in line 33. The developer declared strict_types=1 in line 1 to ensure the the type hint in the validate function in line 7 throws a TypeError exception if a non-int is passed to the class. Even with strict types enabled there is an bug with the usage of array_walk() which ignores the strict typing and uses the default weak typing of PHP instead. An attacker can therefore just append a command to the last parameter that is executed in the system call. A possible payload could look like ?p[1]=1&p[2]=2;%20ls%20-la.

# Refference
+ php-security-calendar-2017 Day 21 - Gift Wrap