FROM ubuntu:16.04

MAINTAINER Kevin Bond <kevinbond@gmail.com>

RUN locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update \

    && apt-get install -y curl zip unzip git software-properties-common \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y php7.0-cli php7.0-fpm \
       php7.0-mysql php7.0-sqlite3 php7.0-curl php7.0-gd php7.0-intl php7.0-sybase php7.0-mbstring \
       php7.0-soap php7.0-xml php7.0-zip php-apcu php-xdebug \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && usermod -u 1000 www-data \
    && groupmod -g 1000 www-data

ADD php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
ADD www.conf /etc/php/7.0/fpm/pool.d/www.conf

EXPOSE 9000
WORKDIR /opt
CMD ["php-fpm7.0"]
