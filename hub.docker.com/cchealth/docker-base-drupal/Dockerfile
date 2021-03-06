FROM php:7.1-apache-jessie

RUN apt-get update && apt-get install -y apt-transport-https ca-certificates libz-dev libmemcached-dev redis-tools unzip && \
    echo "deb https://dl-ssl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list && \
    curl -O https://dl-ssl.google.com/linux/linux_signing_key.pub && \
    apt-key add linux_signing_key.pub && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" >> /etc/apt/sources.list && \
    curl -O https://dl.yarnpkg.com/debian/pubkey.gpg && \
    apt-key add pubkey.gpg && \
    apt-get update && apt-get install -y git zip libbz2-dev build-essential mysql-client libpng12-dev libjpeg-dev google-chrome-stable yarn && \
    pecl install xdebug redis memcached  && docker-php-ext-enable xdebug redis memcached  && \
    docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr && \
    docker-php-ext-install bz2 gd mbstring opcache pdo pdo_mysql zip && \
    a2enmod rewrite && \
    echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.profiler_enable=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.profiler_enable_trigger=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.profiler_output_name = \"cachegrind.out.%t-%s\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    echo "xdebug.profiler_append=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && \
    mv /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini.disabled && \
    echo "opcache.memory_consumption=128" > /usr/local/etc/php/conf.d/opcache-recommended.ini && \
    echo "opcache.interned_strings_buffer=8" >> /usr/local/etc/php/conf.d/opcache-recommended.ini && \
    echo "opcache.max_accelerated_files=4000" >> /usr/local/etc/php/conf.d/opcache-recommended.ini && \
    echo "opcache.revalidate_freq=60" >> /usr/local/etc/php/conf.d/opcache-recommended.ini && \
    echo "opcache.fast_shsutdown=1" >> /usr/local/etc/php/conf.d/opcache-recommended.ini && \
    echo "opcache.enable_cli=1" >> /usr/local/etc/php/conf.d/opcache-recommended.ini && \
    echo "memory_limit=2048M" > /usr/local/etc/php/conf.d/global.ini && \
    echo "date.timezone=Europe/London" >> /usr/local/etc/php/conf.d/global.ini && \
    echo "max_input_vars = 5000" >> /usr/local/etc/php/conf.d/global.ini && \
    echo "max_execution_time = 360" >> /usr/local/etc/php/conf.d/global.ini && \
    echo "max_input_time = 120" >> /usr/local/etc/php/conf.d/global.ini && \
    touch /usr/local/etc/php/php.ini

# Switch to bash
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Pretend that we're vagrant so that BLT doesn't try and initiate SSH tunnels when using drush.
ENV USER vagrant

# Install things from random internet sites
# Highway to the daaanger zooone
RUN curl --silent --show-error https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && \
    curl -sL https://deb.nodesource.com/setup_8.x | sh && apt-get install -y nodejs && \
    composer global require "hirak/prestissimo:^0.3"

