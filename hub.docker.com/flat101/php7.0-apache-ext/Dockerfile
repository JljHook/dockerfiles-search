#
# image: flat101/php7.2.10-apache-ext
# Inherits from php:7.2.10-apache
# https://hub.docker.com/r/flat101/php7.2.10-apache-ext/
#

FROM php:7.0.32-apache

# Install dependencies
RUN apt-get update \
    && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libpq-dev \
        g++ \
        libicu-dev \
        libxml2-dev \
        libmcrypt-dev \
        optipng \
        jpegoptim \
        zip \
    # mcrypt for >=7.2
    # && pecl install mcrypt-1.0.1 \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install zip \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install soap \
    && docker-php-ext-install opcache \
    # mcrypt for <7.2
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-enable mcrypt \
    && apt-get purge --auto-remove -y g++ \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Imagemagick & PHP Imagick ext
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libmagickwand-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick

# Install VIM, MySQL Client
RUN apt-get update \
    && apt-get install -y \
        vim \
        mysql-client

# Install git
RUN apt-get update \
    && apt-get install -y \
        git \
        git-core \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl --output composer-setup.php https://getcomposer.org/installer \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer

# Install xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Install redis
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

# Remove not necessary files
RUN rm -rf /var/lib/apt/lists/*

# Set the timezone.
RUN echo "Europe/Madrid" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# Enable Apache rewrite module
RUN a2enmod rewrite

# Set apache root dir environment variable
ENV APACHE_DOCUMENT_ROOT /var/www/html
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf