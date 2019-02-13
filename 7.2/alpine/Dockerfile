FROM php:7.2-cli-alpine

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
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
  && apk add --no-cache icu freetype libpng libjpeg-turbo \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install gd \
    intl \
    zip \
    pdo_mysql \
    opcache \
    iconv \
  && pecl install xdebug redis \
  && docker-php-ext-enable xdebug \
  && apk del --purge .build-deps \
  && rm -rf /tmp/pear ~/.pearrc /var/tmp/* /tmp/*

COPY --from=composer:1.7 /usr/bin/composer /usr/bin/composer

RUN composer global require hirak/prestissimo --prefer-dist --no-interaction \
  && rm -rf /root/.composer/cache/*

WORKDIR /app
