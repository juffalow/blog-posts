---
title: 'What is and how to use Composer'
media_order: logo-composer.png
date: '28-01-2016 16:50'
taxonomy:
    tag:
        - php
        - Composer
        - 'Example project'
header_image_file: logo-composer.png
---

If you are learning **PHP** or you are experienced programmer you surely read the word **Composer**. But…what is it? What is its purpose and why should you use it? Here I want to write a few things about it and show you how to use it. **PHPUnit** testing framework should meet the requirements as proper and useful example. Let’s get started…

===

## Composer

Let’s have a situation, that you have written a couple of classes that do common tasks like some database management, error logging, etc. Then you create a whole new project where you want to use it, but! You have to write it again or find it somewhere and copy all the files it needs. All these are only your classes so you know what they need for their work. But what if you want to use somebody’s else classes? Or maybe a friend / colleague want to use yours but without your help it would be pain. This is the situation when Composer come in.

#### Usage

So again, you create a new project. Then you write down all dependencies ( packages / classes you need ), run Composer and it will download everything for you. Each of these dependencies might have also dependencies but you do not have to care about it. Composer will pull in all required libraries.

Therefor the Composer is package manager for PHP language. It is strongly inspired by Node.js’s NPM and Ruby’s bundler. Or maybe you know Maven from java which is something similar.

#### composer.json

This is a json file where you set the dependencies and / or other metadata. The main part of this file is the `require` key. You’re simply telling Composer which packages your project depends on. The `require` key takes an object that maps package names to version constraints.

*Composer.json* example from Symfony php framework :

```
{
    "name": "symfony/symfony",
    "type": "library",
    "description": "The Symfony PHP framework",
    "keywords": ["framework"],
    "homepage": "https://symfony.com",
    "license": "MIT",
    "authors": [
        {
            "name": "Fabien Potencier",
            "email": "fabien@symfony.com"
        },
        {
            "name": "Symfony Community",
            "homepage": "https://symfony.com/contributors"
        }
    ],
    "require": {
        "php": ">=5.5.9",
        "doctrine/common": "~2.4",
        "twig/twig": "~1.23|~2.0",
        "psr/cache": "~1.0",
        "psr/log": "~1.0",
        "symfony/polyfill-apcu": "~1.0,>=1.0.2",
        "symfony/polyfill-intl-icu": "~1.0",
        "symfony/polyfill-mbstring": "~1.0",
        "symfony/polyfill-php56": "~1.0",
        "symfony/polyfill-php70": "~1.0",
        "symfony/polyfill-util": "~1.0"
    },
    ...
}
```

The **Package name** consists of a vendor name and the project’s name. The vendor name just exists to prevent naming clashes. It allows two different people to create a library with the same name but with different vendor name like `igorw/json` and `seldaek/json`.

**Package version** is type of constraint for Composer so it will download only certain version or version matching the limitation. ( You can reed more about it in [Composer’s version documentation](https://getcomposer.org/doc/articles/versions.md) )

#### composer.lock

When you run <kbd>composer install</kbd> command it will find versions of required packages that matches the supplied version constraint and download them into *vendor* directory.

After installing the dependencies, Composer writes the list of exact versions it installed into a composer.lock file. You should commit composer.lock file into version control to prevent downloading other versions of the dependencies. It is because the `install` command checks if a lock file is present, and if so, it downloads the versions specified there. It means your server, other developers and everyone else runs the application on the same dependencies.

<div class="alert alert-info" role="alert">If you are using <i>GIT</i> for versioning your code, you can add <i>vendor</i> directory into <i>.gitignore</i>. You don’t need to have all the package’s code in your repository.</div>

#### autoload.php

For libraries that specify autoload information, Composer generates a *vendor/autoload.php* file. You can simply include this file and you will get autoloading for free. This makes it really easy to use required packages.

You can also add your own code to autoloader by adding an `autoload` key to *composer.json*. With this feature you define mapping from namespaces to directories.

## Example project *with PHPUnit*

Create a class to check if given string is [palindrome](https://en.wikipedia.org/wiki/Palindrome) – a sequence of characters which reads the same backward or forward.

Folder structure :

```
ComposerExample
    src
    tests
        PalindromeTest.php
        composer.json
```

#### PHPUnit

As [Test driven development](https://en.wikipedia.org/wiki/Test-driven_development) suggest, we begin with creating proper tests. What do we need… First, we need some testing framework. For this, I use well-known PHPUnit. So add it into *composer.json* :

```
{
    "require": {
        "phpunit/phpunit": "^4.6.0"
    },
    "autoload": {
        "psr-4": {
            "src\\" : "src"
        }
    }
}
```

And run command <kbd>composer install</kbd>. On the output is a list of packages and their versions which were downloaded. They can be found in vendor directory along with *autoload.php* file.

#### Tests

When creating tests, you should think about the problem that should be solved and imagine the design. This part can help you better understand the problem and scenarios that can happen.
Is it all about strings? Might someone want to test if a number is palindrome? Maybe in PHP the variable type is not a problem, but in other languages this does not apply.

Final solution includes class that wraps the functionality and a method to evaluate if given string is palindrome or not. The class name can be `Palindrome` and method `check()`. We need to test both cases. The case the string is palindrome and case it is not. So the test can be written like this :

```
class PalindromeTest extends PHPUnit_Framework_TestCase {

    private $palindrome;
    
    public function setUp() {
        $this->palindrome = new src\Palindrome();
    }

    public function testSuitableString() {
        $this->assertTrue($this->palindrome->check('racecar'));
        $this->assertTrue($this->palindrome->check('was it a car or a cat i saw'));
    }

    public function testNotSuitableString() {
        $this->assertFalse($this->palindrome->check('some meaningless text'));
    }
}
```

Now, when the test is ready, run command <kbd>phpunit --bootstrap ./vendor/autoload.php ./tests/PalindromeTest.php</kbd>.

First argument is `--bootstrap` and its value is path to file which is responsible for autoloading. Second argument is path to test file or to whole directory. PHPUnit will search files that ends with “&ast;test.php” or any other defined string ( more about this on their official website ).

The class does not exist yet, so the test will end with fatal error *Class ‘src\Palindrome’ not found*.

#### Writing the code

The text should be the same from the beginning as from the end. For this, you can use PHP function `strrev` and compare original string with the reverse one. Create a new php file in src directory named *Palindrome.php*.

```
<?php

namespace src;

class Palindrome {
    /**
     * Check if a string is palindrome
     * @param string $str
     * @return bool
     */
    public function check( $str ) {
        return $str == strrev($str);
    }
}
```

Now, if the test is run, no fatal occurs and the output is something like :

```
$ phpunit –bootstrap vendor/autoload.php tests/PalindromeTest.php
PHPUnit 4.8.21 by Sebastian Bergmann and contributors.
F.

Time: 263 ms, Memory: 4.00Mb

There was 1 failure:

1) PalindromeTest::testSuitableString
Failed asserting that false is true.

*/ComposerExample/tests/PalindromeTest.php:15

FAILURES!
Tests: 2, Assertions: 3, Failures: 1.
```

The test ended with *FAILURES!* result. One test did not pass and you can find it on line 15. When you look at that line it is obvious why it is not the same string from the backwards. There are spaces between words therefore it is not equal. You have to get rid of the spaces. Code after edit :

```
public function check( $str ) {
    $sanitizedString = \str_replace(' ', '', $str);
    return $sanitizedString == strrev($sanitizedString);
}
```

Run test again and it should successfully pass each test.

```
OK (2 tests, 4 assertions)
```

Of course this should not be the end. There are always strings which should pass but would not. For example if there is uppercase letter or some redundant character like dot or question mark. If you spend a little bit more time thinking about the problem and mentioned scenarios, while you are writing tests, you can prevent further problems.

## Conclusion

This is the end of short Composer intro. I hope everything important was mentioned. If you cannot understand something, feel free to contact me.
There are still many things I did not write about. You are able to define extra dependencies for dev environment, or you can create your own ( private ) Composer repository. The same it is with PHPUnit framework, which has a lot other functions. More about it in some later post.

#### References

[Composer](https://getcomposer.org/) official website, where you can find *Getting Started* tutorial, documentation, packages and other information.

[PHPUnit testing framework](https://phpunit.de/) official website with great documentation.

[Symfony framework](https://github.com/symfony/symfony) page on GitHub, where you can find whole *composer.json* file.
