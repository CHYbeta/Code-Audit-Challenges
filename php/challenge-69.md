# Challenge
```php 
class Mailer {
    private function sanitize($email) {
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            return '';
        }

        return escapeshellarg($email);
    }

    public function send($data) {
        if (!isset($data['to'])) {
            $data['to'] = 'none@ripstech.com';
        } else {
            $data['to'] = $this->sanitize($data['to']);
        }

        if (!isset($data['from'])) {
            $data['from'] = 'none@ripstech.com';
        } else {
            $data['from'] = $this->sanitize($data['from']);
        }

        if (!isset($data['subject'])) {
            $data['subject'] = 'No Subject';
        }

        if (!isset($data['message'])) {
            $data['message'] = '';
        }

        mail($data['to'], $data['subject'], $data['message'],
             '', "-f" . $data['from']);
    }
}

$mailer = new Mailer();
$mailer->send($_POST);
```

# Solution
This challenge suffers from a command execution vulnerability in line 31. The fifth parameter of mail, in this case the variable $_POST['from'], is appended to the sendmail command that is executed to send out the email. It is not possible to execute arbitrary commands here but it is possible to append arbitrary new parameters to sendmail. This can be abused to create a PHP backdoor in the web directory through the log files of sendmail.
There are 2 insufficient protections in place that try to prevent successful exploitation. The method sanitize() first checks in line 3 if the e-mail address is valid. However, not all characters that are necessary to exploit the security issue in mail() are forbidden by this filter. It allows the usage of escaped whitespaces nested in double quotes. In line 7 the e-mail address gets sanitized with escapeshellarg(). This would be sufficient if PHP would not escape the fifth parameter internally with escapeshellcmd(). Since it does escape the parameter again, the escapeshellcmd() allows an attacker to break out of the escapeshellarg(). More information, details, and a PoC can be found in our blog post “Why mail() is dangerous in PHP”.

# Refference
+ php-security-calendar-2017 Day 5 - Postcard