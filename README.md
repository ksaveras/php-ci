# PHP CI Docker images

* [7.4-buster](https://github.com/ksaveras/php-ci/blob/master/7.4/buster/Dockerfile)
* [7.4-alpine](https://github.com/ksaveras/php-ci/blob/master/7.4/alpine/Dockerfile)
* [7.3-stretch](https://github.com/ksaveras/php-ci/blob/master/7.3/stretch/Dockerfile)
* [7.3-alpine](https://github.com/ksaveras/php-ci/blob/master/7.3/alpine/Dockerfile)
* [7.2-alpine](https://github.com/ksaveras/php-ci/blob/master/7.2/alpine/Dockerfile)
* [7.1-stretch](https://github.com/ksaveras/php-ci/blob/master/7.1/stretch/Dockerfile)
* [7.1-alpine](https://github.com/ksaveras/php-ci/blob/master/7.1/alpine/Dockerfile)

This repository extends official PHP Docker image with production 
PHP ini settings and adds often used PHP extensions.
This allows quickly integrate project in PHP CI processes.

## Additional tools
* GIT
* composer (2.2 from official Docker image)
* phpcs, phpcbf - PHP_CodeSniffer (with Symfony coding standards)
* phpcpd - Copy/Paste Detector (CPD)
* phploc - Measure PHP project code size
* phpmd - PHP Mess Detector
* pdepend - Software Metrics for PHP
* php-cs-fixer - PHP Coding Standards Fixer
* phpstan - PHP Static Analysis Tool
  * phpstan/phpstan-doctrine
  * phpstan/phpstan-phpunit
  * phpstan/phpstan-symfony

## PHP Extensions

By default xdebug extension is disabled for speed.
If you want to use php with xdebug - use `php-xdebug` command
or enable xdebug extension with `-d` flag: `php -dzend_extension=xdebug.so ... rest of arguments`

| Extension | PHP 7.1 | PHP 7.2 | PHP 7.3 | PHP 7.4 |
|---|:---:|:---:|:---:|:---:|
|bcmath| + | + | + | + |
|ctype| + | + | + | + |
|curl| + | + | + | + |
|date| + | + | + | + |
|dom| + | + | + | + |
|fileinfo| + | + | + | + |
|filter| + | + | + | + |
|ftp| + | + | + | + |
|gd| + | + | + | + |
|hash| + | + | + | + |
|iconv| + | + | + | + |
|intl| + | + | + | + |
|json| + | + | + | + |
|libxml| + | + | + | + |
|mbstring| + | + | + | + |
|mcrypt| + |  |  |  |
|mysqlnd| + | + | + | + |
|openssl| + | + | + | + |
|pcre| + | + | + | + |
|PDO| + | + | + | + |
|pdo_mysql| + | + | + | + |
|pdo_sqlite| + | + | + | + |
|Phar| + | + | + | + |
|posix| + | + | + | + |
|readline| + | + | + | + |
|redis| + | + | + | + |
|Reflection| + | + | + | + |
|session| + | + | + | + |
|SimpleXML| + | + | + | + |
|soap| + | + | + | + |
|sockets| + | + | + | + |
|sodium|  | + | + | + |
|SPL| + | + | + | + |
|sqlite3| + | + | + | + |
|standard| + | + | + | + |
|tokenizer| + | + | + | + |
|xdebug| + (not enabled) | + (not enabled) | + (not enabled) | + (not enabled) |
|xml| + | + | + | + |
|xmlreader| + | + | + | + |
|xmlwriter| + | + | + | + |
|Zend OPcache| + | + | + | + |
|zip| + | + | + | + |
|zlib| + | + | + | + |


## License

The Dockerfiles are licensed under the MIT license, see LICENSE for details.
