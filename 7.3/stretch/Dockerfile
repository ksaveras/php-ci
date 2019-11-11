FROM php:7.3-cli-stretch

RUN apt-get update \
  && set -xe \
  && apt-get install -y \
      automake \
      libtool \
      libpng-dev \
      libssl-dev \
      libcurl4-openssl-dev \
      pkg-config \
      libedit-dev \
      libxml2-dev \
      libjpeg-dev \
      libpng-dev \
      libfreetype6-dev \
      libzip-dev \
    wget \
    curl \
    git \
    unzip \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install gd \
    bcmath \
    intl \
    zip \
    pdo_mysql \
    opcache \
    soap \
    sockets \
    iconv \
  && pecl install xdebug-beta redis \
  && docker-php-ext-enable redis \
  && rm -rf /tmp/pear ~/.pearrc /var/tmp/* /tmp/*

RUN { \
        echo '#!/bin/sh'; \
        echo '/usr/local/bin/php -dzend_extension=xdebug.so "$@"'; \
    } > /usr/local/bin/php-xdebug \
    && chmod +x /usr/local/bin/php-xdebug

COPY --from=composer:1.7 /usr/bin/composer /usr/bin/composer

RUN composer global require \
    hirak/prestissimo \
    phpmd/phpmd \
    pdepend/pdepend \
    phpstan/phpstan \
    phpstan/phpstan-doctrine \
    phpstan/phpstan-phpunit \
    phpstan/phpstan-symfony \
    --prefer-dist --no-interaction \
    && rm -rf /root/.composer/cache/*

RUN wget -c https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -O /usr/local/bin/phpcs \
    && chmod a+x /usr/local/bin/phpcs

RUN wget -c https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar -O /usr/local/bin/phpcbf \
    && chmod a+x /usr/local/bin/phpcbf

RUN mkdir -p /usr/local/etc/phpcs \
    && git clone git://github.com/escapestudios/Symfony2-coding-standard.git /usr/local/etc/phpcs/Symfony2-coding-standard \
    && /usr/local/bin/phpcs --config-set installed_paths /usr/local/etc/phpcs/Symfony2-coding-standard

RUN wget -c https://phar.phpunit.de/phpcpd.phar -O /usr/local/bin/phpcpd \
    && chmod a+x /usr/local/bin/phpcpd

RUN wget -c https://phar.phpunit.de/phploc.phar -O /usr/local/bin/phploc \
    && chmod a+x /usr/local/bin/phploc

RUN wget -c https://cs.symfony.com/download/php-cs-fixer-v2.phar -O /usr/local/bin/php-cs-fixer \
    && chmod a+x /usr/local/bin/php-cs-fixer

RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

ENV PATH="$PATH:/root/.composer/vendor/bin"

WORKDIR /app
