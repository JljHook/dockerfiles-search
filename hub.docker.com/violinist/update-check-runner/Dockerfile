FROM drupaldocker/php:7.2-cli-2.x
MAINTAINER eiriksm <eirik@morland.no>
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp

ENV COMPOSER_DISCARD_CHANGES=1
ENV COMPOSER_MEMORY_LIMIT=-1
ENV COMPOSER_ALLOW_SUPERUSER=1

RUN composer self-update \
    && composer install --no-dev --optimize-autoloader \
    && composer require bitbucket/client \
    && composer global require hirak/prestissimo \
    && apk add --no-cache imagemagick imagemagick-libs imagemagick-dev autoconf bzip2-dev g++ make icu-dev \
    && yes | pecl install apcu mongodb imagick \
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-install bz2 pcntl intl \
    && docker-php-ext-enable apcu intl mongodb imagick

RUN git clone https://github.com/FriendsOfPHP/security-advisories /root/.symfony/cache/security-advisories
RUN git clone https://github.com/violinist-dev/drupal-contrib-sa /root/drupal-contrib-sa
RUN echo "DRUPAL_CONTRIB_SA_PATH=/root/drupal-contrib-sa" > /usr/src/myapp/.env
RUN wget https://get.symfony.com/cli/v4.5.4/symfony_linux_amd64 -O /tmp/symfony.gz
RUN gzip -d /tmp/symfony.gz
RUN chmod 755 /tmp/symfony
RUN mv /tmp/symfony /usr/local/bin/symfony
CMD ["php", "runner.php"]
