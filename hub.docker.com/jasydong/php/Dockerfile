FROM php:7.2-fpm

RUN apt-get update \
    && apt-get install -y git unzip wget sudo vim libpng-dev libjpeg-dev libbz2-dev libicu-dev libmcrypt-dev libpq-dev libmagickwand-dev

RUN docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
    && docker-php-ext-install bcmath \
        bz2 \
        exif \
        gettext \
        gd \
        intl \
        opcache \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        pgsql \
        soap \
        sockets \
        zip

RUN pecl install Imagick && echo "extension=imagick.so" > /usr/local/etc/php/conf.d/imagick.ini
RUN pecl install xdebug && echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini
RUN pecl install mcrypt-1.0.1 && echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini
RUN pecl install yaf && echo "extension=yaf.so" > /usr/local/etc/php/conf.d/yaf.ini

RUN sudo apt-get install -y libevent-dev libssl-dev \
    && sudo pecl install event \
    && echo "extension=event.so" > /usr/local/etc/php/conf.d/event.ini

RUN cd /usr/local/lib \
	&& wget https://composer.github.io/installer.sig -O - -q | tr -d '\n' > installer.sig \
	&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
	&& php -r "if (hash_file('SHA384', 'composer-setup.php') === file_get_contents('installer.sig')) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
	&& php composer-setup.php \
	&& php -r "unlink('composer-setup.php'); unlink('installer.sig');" \
	&& mv /usr/local/lib/composer.phar /usr/local/bin/composer

RUN chmod g+w /usr/local/etc/php/conf.d/ \
    && usermod -a -G staff www-data \
    && chown www-data:staff /var/www/html \
    && echo 'www-data  ALL=(ALL:ALL) NOPASSWD: ALL' > /etc/sudoers.d/www-data

USER www-data
WORKDIR /var/www/html

EXPOSE 8080
