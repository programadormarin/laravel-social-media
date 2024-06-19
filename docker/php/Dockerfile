FROM php:8.3.8-fpm-alpine

RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

RUN apk add --no-cache mysql-client msmtp perl wget procps shadow libzip \
    libpng libjpeg-turbo freetype freetype-dev libwebp freetype libpng \
    libjpeg-turbo-dev icu icu-data-full

RUN apk add --no-cache --virtual build-essentials \
    icu-dev icu-libs zlib-dev g++ make automake autoconf libzip-dev \
    libpng-dev libwebp-dev libjpeg-turbo-dev freetype-dev && \
    docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp && \
    docker-php-ext-install gd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install intl && \
    docker-php-ext-install opcache && \
    docker-php-ext-install exif && \
    docker-php-ext-install zip && \
    docker-php-ext-install pdo && \
    docker-php-ext-install pdo_mysql && \
    apk del build-essentials && rm -rf /usr/src/php*
RUN apk add --update linux-headers

RUN apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

RUN apk add --no-cache $PHPIZE_DEPS \
	&& pecl install xdebug \
	&& docker-php-ext-enable xdebug

RUN wget https://getcomposer.org/composer-stable.phar -O /usr/local/bin/composer && chmod +x /usr/local/bin/composer

# Create folder to run
RUN mkdir -p /var/www/html

# Set Profile
RUN chown laravel:laravel /var/www/html

# Work in the specific space
WORKDIR /var/www/html

