FROM php:7.4-cli-alpine

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
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libzip-dev \
  && apk add --no-cache git unzip icu freetype libzip libpng libjpeg-turbo \
  && docker-php-ext-configure gd --with-freetype \
    --with-jpeg \
  && docker-php-ext-install gd \
    bcmath \
    intl \
    zip \
    pdo_mysql \
    opcache \
    soap \
    sockets \
    iconv \
  && pecl install xdebug redis \
  && docker-php-ext-enable redis \
  && apk del --purge .build-deps \
  && rm -rf /tmp/pear ~/.pearrc /var/tmp/* /tmp/*

RUN { \
        echo '#!/bin/sh'; \
        echo '/usr/local/bin/php -dzend_extension=xdebug.so "$@"'; \
    } > /usr/local/bin/php-xdebug \
    && chmod +x /usr/local/bin/php-xdebug

COPY --from=composer:1.10 /usr/bin/composer /usr/bin/composer

RUN composer global require \
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

RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" \
    && sed -i "s/memory_limit\s*=.*/memory_limit=-1/g" "$PHP_INI_DIR/php.ini"

ENV PATH="$PATH:/root/.composer/vendor/bin"

WORKDIR /app
