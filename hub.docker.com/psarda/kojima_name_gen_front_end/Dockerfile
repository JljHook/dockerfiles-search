FROM php:7.2-apache

RUN apt-get update
RUN apt-get install -y libfreetype6-dev
RUN apt-get install -y libjpeg62-turbo-dev
RUN apt-get install -y libpng-dev

RUN docker-php-ext-install -j$(nproc) iconv 

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ 

RUN docker-php-ext-install -j$(nproc) gd

COPY . /var/www/html/