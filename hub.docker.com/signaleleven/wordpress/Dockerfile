ARG PHPVERSION
FROM php:7.1-apache
ENV TERM=xterm
LABEL maintainer="Michael Sacchi <michael.sacchi@isaa.it>"

# Install base requirements & sensible defaults + required PHP extensions
RUN echo "deb http://ftp.debian.org/debian jessie-backports main" >> /etc/apt/sources.list \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        less \
        libpng12-dev \
        libjpeg-dev \
        libxml2-dev \
        mariadb-client \
        unzip \
        sudo \
        vim \
        zip \
    && DEBIAN_FRONTEND=noninteractive apt-get -t jessie-backports install -y \
        python-certbot-apache \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
    && docker-php-ext-install \
        exif \
        gd \
        mysqli \
        opcache \
        soap \
        zip \
    && echo 'memory_limit = 512M' > /usr/local/etc/php/php.ini \
    && { \
        echo 'opcache.memory_consumption=128'; \
        echo 'opcache.interned_strings_buffer=8'; \
        echo 'opcache.max_accelerated_files=4000'; \
        echo 'opcache.revalidate_freq=2'; \
        echo 'opcache.fast_shutdown=1'; \
        echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini

# Install wp-cli, configure apache, add scripts, create install directory & symlink
COPY ./run.sh /run.sh
RUN curl -s \
        -o /usr/local/bin/wp https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x /usr/local/bin/wp /run.sh \
    && curl -s \
        https://raw.githubusercontent.com/wp-cli/wp-cli/master/utils/wp-completion.bash | \
        sed -e "s/wp cli completions/wp cli completions --allow-root/" > /etc/bash_completion.d/wp-cli \
    && sed -i 's/AllowOverride None/AllowOverride All/g' /etc/apache2/apache2.conf \
    && sed -i 's/LogFormat \"%h/LogFormat \"%a/g' /etc/apache2/apache2.conf \
    && echo "RemoteIPHeader X-Forwarded-For" > /etc/apache2/conf-available/remoteip.conf \
    && echo "RemoteIPInternalProxy 172.0.0.0/8" >>  /etc/apache2/conf-available/remoteip.conf && a2enconf remoteip \
    && echo "ServerName localhost" | tee /etc/apache2/conf-available/fqdn.conf && a2enconf fqdn \
    && a2enmod rewrite expires remoteip \
    && service apache2 restart \
    && mkdir -p /app ~/.wp-cli \
    && rm -fr /var/www/html \
    && ln -s /app /var/www/html

WORKDIR /app
EXPOSE 80 443
CMD ["/run.sh"]
