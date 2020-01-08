FROM php:7.3.5-fpm

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN echo "deb http://ftp.uk.debian.org/debian stretch-backports main" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y procps zip git mysql-client pkg-config libssl-dev locate vim libzip-dev ffmpeg wget bc axel nodejs aria2 nginx supervisor


# Install GD
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd && docker-php-ext-install pdo && docker-php-ext-install pdo_mysql


#Install redis
ENV PHPREDIS_VERSION 3.0.0
RUN mkdir -p /usr/src/php/ext/redis \
    && curl -L https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz | tar xvz -C /usr/src/php/ext/redis --strip 1 \
    && echo 'redis' >> /usr/src/php-available-exts \
    && docker-php-ext-install redis

 
#Install upload progress
RUN mkdir -p /usr/src/php/ext/uploadprogress; \
    up_url="https://github.com/wodby/pecl-php-uploadprogress/archive/latest.tar.gz"; \
    wget -qO- "${up_url}" | tar xz --strip-components=1 -C /usr/src/php/ext/uploadprogress; \
    docker-php-ext-install uploadprogress   

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo  xdebug.idekey = \"PHPSTORM\" >> /usr/local/etc/php/conf.d/xdebug.ini

#aliases
RUN alias drush='php /var/www/html/vendor/bin/drush' \
    && alias drupal='php /var/www/html/drupal.phar' \
    && alias composer='php /var/www/html/composer.phar'

RUN yes | pecl install zip  \
  && echo "extension=$(find /usr/local/lib/php/extensions/ -name zip.so)" > /usr/local/etc/php/conf.d/zip.ini

RUN pear install XML_RPC2

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/usr/local/bin/ --filename=composer
RUN php -r "unlink('composer-setup.php');"


# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

COPY supervisord.conf /supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/supervisord.conf"]
