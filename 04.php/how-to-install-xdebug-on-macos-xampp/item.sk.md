---
title: 'How to install XDebug on MacOS (XAMPP)'
published: false
taxonomy:
    tag:
        - php
---

When you want to create *codecoverage* report with **phpunit**, you need to have XDebug installed otherwise you get just error "No code coverage driver is available". I successfully installed it on Windows but I had lot of issues installing it on MacOS.

===

## Steps

* check lot of websites and see that there is still new and new issue
* prepare everything
* clone xdebug project
* build xdebug
* install
* enable xdebug module

## Prepare everything

MacOS has php installed with xcode, but I needed to use newer version I am already using with XAMPP. So I just created a link to XAMPP version:

```shell
sudo ln /Applications/XAMPP/bin/php /usr/local/bin/php
```

In XDebug readme is, that you need to use **phpize**. After I linked that one, it had another problem - "fatal error: 'php.h' file not found". To solve also this problem, you have to link **php-config** either.

```shell
sudo ln /Applications/XAMPP/bin/phpize /usr/local/bin/phpize

sudo ln /Applications/XAMPP/bin/php-config /usr/local/bin/php-config
```

## Clone XDebug project

You need to clone the source code, so that you are able to build it:

```shell
git clone git://github.com/xdebug/xdebug.git
```

## Build XDebug and install it

```shell
phpize

make clean

make

make install
```
## Enable XDebug module

The last step is to add *zend_extension="xdebug.so"* in the end of *php.ini* file. If you do not know where this file is located, just run `php --ini`.
