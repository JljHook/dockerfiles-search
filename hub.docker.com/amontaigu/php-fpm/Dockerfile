# ================================================================================================================
#
# PHP-FPM
#
# My personal derivation from official php fpm-alpine alpine image including
# - default configuration
# - main common extension for most of PHP projects
#
# @see https://github.com/docker-library/php/issues/225
# @see https://github.com/docker-library/php/issues/326
# @see https://github.com/m2sh/php7/blob/master/alpine/Dockerfile
# @see https://medium.com/@shrikeh/setting-up-nginx-and-php-fpm-in-docker-with-unix-sockets-6fdfbdc19f91
# ================================================================================================================

# Base is official php image
FROM php:7.1.11-fpm-alpine

# Maintainer
MAINTAINER alban.montaigu@gmail.com

# Install prerequisities
RUN apk add --no-cache \
                pcre-dev \
                freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
                imagemagick imagemagick-dev \
                libmcrypt libmcrypt-dev \
                zlib zlib-dev \
                icu-dev \
                gettext-dev && \

# Pecl php ext based install
    apk add --no-cache --virtual .imagick-build-dependencies libtool make autoconf gcc g++ && \
    pecl install imagick && \
    docker-php-ext-enable imagick && \
    apk del --no-cache .imagick-build-dependencies && \

# Configure gd extension for folling install
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \

# Docker php ext install based extensions in one shot for optim
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    docker-php-ext-install -j${NPROC} gd mcrypt zip intl gettext mbstring mysqli pdo_mysql exif opcache && \

# Install composer since more php apps require it
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Custom php configuration files
COPY ./conf/php/php.ini $PHP_INI_DIR/
COPY ./conf/php/php-cli.ini $PHP_INI_DIR/
COPY ./conf/php-fpm.d/*.conf /usr/local/etc/php-fpm.d/
COPY ./conf/php-fpm.conf /usr/local/etc/

# Volumes to share
VOLUME ["/var/www", "/var/run/php"]
WORKDIR /var/www

