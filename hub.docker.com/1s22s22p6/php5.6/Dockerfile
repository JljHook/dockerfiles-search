FROM php:5.6-fpm
RUN apt-get update \
    && apt-get install -y zlib1g-dev cron telnet vim libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng12-dev libmemcached-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt mysqli mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && printf "\n" | pecl install memcache \
    && docker-php-ext-enable memcache gd iconv mcrypt mysqli mbstring
RUN echo 'extension=memcache.so' > /usr/local/etc/php/conf.d/docker-php-ext-memcached.ini
