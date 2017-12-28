# Challenge 
```php 
// composer require "twig/twig"
require 'vendor/autoload.php';

class Template {
    private $twig;

    public function __construct() {
        $indexTemplate = '<img ' .
            'src="https://loremflickr.com/320/240">' .
            '<a href="{{link|escape}}">Next slide »</a>';

        // Default twig setup, simulate loading
        // index.html file from disk
        $loader = new Twig\Loader\ArrayLoader([
            'index.html' => $indexTemplate
        ]);
        $this->twig = new Twig\Environment($loader);
    }

    public function getNexSlideUrl() {
        $nextSlide = $_GET['nextSlide'];
        return filter_var($nextSlide, FILTER_VALIDATE_URL);
    }

    public function render() {
        echo $this->twig->render(
            'index.html',
            ['link' => $this->getNexSlideUrl()]
        );
    }
}

(new Template())->render();
```

# Solution 
The challenge contains a cross-site scripting vulnerability in line 26. There are two filters that try to assure that the link that is passed to the <a> tag is a genuine URL. First, the filter_var() function in line 22 checks if it is a valid URL. Then, Twig's template escaping is used in line 10 that avoids breaking out of the href attribute.

The vulnerability can still be exploited with the following URL: ?nextSlide=javascript://comment%250aalert(1).
The payload does not involve any markup characters that would be affected by Twig's escaping. At the same time, it is a valid URL for filter_var(). We used a JavaScript protocol handler, followed by a JavaScript comment introduced with // and then the actual JS payload follows on a newline. When the link is clicked, the JavaScript payload is executed in the browser of the victim.

# Refference 
+ php-security-calendar-2017 Day 2 - Twig