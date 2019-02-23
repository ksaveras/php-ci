FROM php:7.1-cli-alpine

RUN apk update && apk upgrade \
  && set -xe \
  && apk add --no-cache --virtual .build-deps \
    icu-dev \
    zlib-dev \
    icu-libs \
    g++ \
    make \
    autoconf \
    libxslt-dev \
    zlib \
    wget \
    curl \
    libtool \
    libcurl \
    openssl-dev \
    libmcrypt-dev \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
  && apk add --no-cache git icu libmcrypt freetype libpng libjpeg-turbo \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install gd \
    mcrypt \
    intl \
    zip \
    pdo_mysql \
    opcache \
    iconv \
  && pecl install xdebug redis \
  && docker-php-ext-enable xdebug \
  && apk del --purge .build-deps \
  && rm -rf /tmp/pear ~/.pearrc /var/tmp/* /tmp/* \
  && mv /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini.disabled

RUN { \
        echo '#!/bin/sh'; \
        echo '/usr/local/bin/php -dzend_extension=xdebug.so "$@"'; \
    } > /usr/local/bin/php-xdebug \
    && chmod +x /usr/local/bin/php-xdebug

COPY --from=composer:1.7 /usr/bin/composer /usr/bin/composer

RUN composer global require \
    hirak/prestissimo \
    escapestudios/symfony2-coding-standard \
    --prefer-dist --no-interaction \
    && rm -rf /root/.composer/cache/*

RUN wget -c https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -O /usr/local/bin/phpcs \
    && chmod a+x /usr/local/bin/phpcs

RUN wget -c https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar -O /usr/local/bin/phpcbf \
    && chmod a+x /usr/local/bin/phpcbf

RUN mkdir -p /usr/local/etc/phpcs \
    && git clone git://github.com/escapestudios/Symfony2-coding-standard.git /usr/local/etc/phpcs/Symfony2-coding-standard \
    && /usr/local/bin/phpcs --config-set installed_paths /usr/local/etc/phpcs/Symfony2-coding-standard

RUN wget -c https://static.phpmd.org/php/latest/phpmd.phar -O /usr/local/bin/phpmd \
    && chmod a+x /usr/local/bin/phpmd

RUN wget -c https://phar.phpunit.de/phpcpd.phar -O /usr/local/bin/phpcpd \
    && chmod a+x /usr/local/bin/phpcpd

RUN wget -c https://phar.phpunit.de/phploc.phar -O /usr/local/bin/phploc \
    && chmod a+x /usr/local/bin/phploc

RUN wget -c https://static.pdepend.org/php/latest/pdepend.phar -O /usr/local/bin/pdepend \
    && chmod a+x /usr/local/bin/pdepend

RUN wget -c https://cs.symfony.com/download/php-cs-fixer-v2.phar -O /usr/local/bin/php-cs-fixer \
    && chmod a+x /usr/local/bin/php-cs-fixer

RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

WORKDIR /app
