# Challenge
```php 
class LanguageManager
{
    public function loadLanguage()
    {
        $lang = $this->getBrowserLanguage();
        $sanitizedLang = $this->sanitizeLanguage($lang);
        require_once("/lang/$sanitizedLang");
    }

    private function getBrowserLanguage()
    {
        $lang = $_SERVER['HTTP_ACCEPT_LANGUAGE'] ?? 'en';
        return $lang;
    }

    private function sanitizeLanguage($language)
    {
        return str_replace('../', '', $language);
    }
}

(new LanguageManager())->loadLanguage();
```

# Solution
This challenge contains a file inclusion vulnerability that can allow an attacker to execute arbitrary code on the server or to leak sensitive files. The bug is in the sanitization function in line 18. The replacement of the ../ string is not executed recursively. This allows the attacker to simply use the character sequence ....// or ..././ that after replacement will end in ../ again. Thus, changing the path to the included language file via path traversal is possible. For example, the system's passwd file can be leaked by setting the following payload in the Accept-Language HTTP request header: .//....//....//etc/passwd.

# Refference
+ php-security-calendar-2017 Day 9 - Rabbit