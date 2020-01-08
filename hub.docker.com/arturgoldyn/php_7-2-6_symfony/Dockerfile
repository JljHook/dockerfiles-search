FROM php:7.2.6
RUN apt-get update -yqq
RUN apt-get install -yqq git libmcrypt-dev libpq-dev libcurl4-gnutls-dev libicu-dev libvpx-dev libjpeg-dev libpng-dev libxpm-dev zlib1g-dev libfreetype6-dev libxml2-dev libexpat1-dev libbz2-dev libgmp3-dev libldap2-dev unixodbc-dev libsqlite3-dev libaspell-dev libsnmp-dev libpcre3-dev libtidy-dev unzip
RUN docker-php-ext-install gmp bcmath zip pdo_mysql mbstring curl json intl gd xml bz2 opcache
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN curl -sS https://getcomposer.org/installer | php
RUN chmod +x composer.phar
RUN mv /composer.phar /usr/bin/composer.phar
RUN ln /usr/bin/composer.phar /usr/bin/composer
RUN apt-get install -yqq python
RUN adduser --disabled-password -gecos "" application
RUN su - application -c "composer global require hirak/prestissimo"
RUN apt-get install -yqq libmemcached-dev
RUN pecl install memcached
RUN echo "extension=memcached.so" > /usr/local/etc/php/conf.d/20_memcached.ini
