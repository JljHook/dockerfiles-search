﻿FROM php:5.6-apache
MAINTAINER oldiy <oldiy2018@gmail.com>

ENV VERSION 2.18
# 删除SUID
RUN for i in `find / -perm +6000 -type f`; do chmod a-s $i; done

# selfoss requirements: mod-headers, mod-rewrite, gd
RUN a2enmod headers rewrite && \
    apt-get update && \
    apt-get install -y unzip libjpeg62-turbo-dev libpng-dev libpq-dev && \
    docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install gd mbstring pdo_pgsql pdo_mysql

ADD https://github.com/SSilence/selfoss/releases/download/${VERSION}/selfoss-${VERSION}.zip /tmp/
RUN unzip /tmp/selfoss-*.zip -d /var/www/html && \
    rm /tmp/selfoss-*.zip && \
    mkdir -p /var/www/html/conf && \
    mkdir -p /conf-copy && \
    chown -R www-data:www-data /var/www/html && \
    apt-get install -yf bash

COPY start.sh /conf-copy/
COPY config.ini /conf-copy/
COPY php.ini /usr/local/etc/php/

RUN chmod +x /conf-copy/start.sh

WORKDIR /
VOLUME ["/var/www/html/conf"]

CMD ["/conf-copy/start.sh"]
