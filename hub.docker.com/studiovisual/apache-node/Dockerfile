FROM php:7.1-apache
MAINTAINER Studio Visual <atendimento@studiovisual.com.br>

ENV COMPOSER_ALLOW_SUPERUSER 1

RUN apt-get update
RUN apt-get install -y apt-utils

# Install GD
RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng12-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

# Install MCrypt
RUN apt-get update \
    && apt-get install -y libmcrypt-dev \
    && docker-php-ext-install mcrypt

# Install Intl
RUN apt-get update \
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl

ENV XDEBUG_ENABLE 0
RUN pecl config-set preferred_state beta \
    && pecl install -o -f xdebug \
    && rm -rf /tmp/pear \
    && pecl config-set preferred_state stable
COPY ./99-xdebug.ini.disabled /usr/local/etc/php/conf.d/

# Install Mysql
RUN docker-php-ext-install mysqli pdo_mysql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# Install mbstring
RUN docker-php-ext-install mbstring

# Install soap
RUN apt-get update \
    && apt-get install -y libxml2-dev \
    && docker-php-ext-install soap

# Install opcache
RUN docker-php-ext-install opcache

# Install PHP zip extension
RUN docker-php-ext-install zip

# Install Git
RUN apt-get update \
    && apt-get install -y git

# Install xsl
RUN apt-get update \
    && apt-get install -y libxslt-dev \
    && docker-php-ext-install xsl

# Define PHP_TIMEZONE env variable
ENV PHP_TIMEZONE Europe/Rome

# Configure Apache Document Root
ENV APACHE_DOC_ROOT /var/www/html

# Enable Apache mod_rewrite
RUN a2enmod rewrite

# Additional PHP ini configuration
COPY ./999-php.ini /usr/local/etc/php/conf.d/

COPY ./index.php /var/www/html/index.php

# Install ssmtp Mail Transfer Agent
RUN apt-get update \
    && apt-get install -y ssmtp \
    && apt-get clean \
    && echo "FromLineOverride=YES" >> /etc/ssmtp/ssmtp.conf \
    && echo 'sendmail_path = "/usr/sbin/ssmtp -t"' > /usr/local/etc/php/conf.d/mail.ini

# Install MySQL CLI Client
RUN apt-get update \
    && apt-get install -y mysql-client


########################################################################################################################

RUN apt-get update
RUN apt-get install --yes curl
RUN curl --silent --location https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install --yes nodejs
RUN apt-get install --yes build-essential

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn

########################################################################################################################

# Start!
COPY ./start /usr/local/bin/
CMD ["start"]
