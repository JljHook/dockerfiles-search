FROM ubuntu:16.04

LABEL maintainer="admin@krishtechnolabs.com"

ENV DEBIAN_FRONTEND=noninteractive 

RUN apt-get update && apt-get install -y --no-install-recommends software-properties-common python-software-properties apt-transport-https curl zip language-pack-en-base nginx git \
    && apt-get update \
    && locale-gen en_US.UTF-8 \
    && export LANG=en_US.UTF-8 \
    && LC_ALL=en_US.UTF-8 \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y --no-install-recommends php7.0 php7.0-fpm php7.0-cli php7.0-common php7.0-gd php7.0-mysql php7.0-mcrypt php7.0-curl php7.0-intl php7.0-xsl php7.0-mbstring php7.0-zip php7.0-bcmath php7.0-iconv php7.0-soap \
    && apt-get remove -y --purge software-properties-common python-software-properties \
    && rm -rf /var/lib/apt/lists/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf \
    && apt-get clean \
    && useradd -m -d /var/www/html/magento -u 1001 magento \
    && usermod -G magento www-data \
    && curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/bin \
    --filename=composer \
    && composer global require hirak/prestissimo --no-plugins --no-scripts \
    && mkdir -p /var/www/html/magento/webroot \
    && mkdir /run/php
