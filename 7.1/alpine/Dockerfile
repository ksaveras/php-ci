FROM php:7.1-cli-alpine

RUN apk update && apk upgrade \
  && set -xe \
  && apk add --no-cache --virtual .build-deps \
    icu-dev \
    zlib-dev \
    git \
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
  && apk add --no-cache icu libmcrypt freetype libpng libjpeg-turbo \
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
  && rm -rf /tmp/pear ~/.pearrc /var/tmp/* /tmp/*

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
  && composer global require hirak/prestissimo --prefer-dist --no-interaction \
  && rm -rf /root/.composer/cache/*

WORKDIR /app
