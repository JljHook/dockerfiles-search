FROM ubuntu:16.04

ENV LANG="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8" \
    TERM="xterm" \
    DEBIAN_FRONTEND="noninteractive" \
    SYMFONY_ALLOW_APPDEV=1 \
    COMPOSER_ALLOW_SUPERUSER=1 \
    NODE_VERSION=6.11.5

EXPOSE 80
WORKDIR /app

RUN apt-get update -q && \
    apt-get install -qy software-properties-common language-pack-en-base && \
    export LC_ALL=en_US.UTF-8 && \
    export LANG=en_US.UTF-8 && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update -q && \
    apt-get install --no-install-recommends -qy \
        curl \
        nano \
        vim \
        nginx \
        git \
        php7.1 \
        php7.1-bcmath \
        php7.1-common \
        php7.1-curl \
        php7.1-dom \
        php7.1-fpm \
        php7.1-intl \
        php7.1-json \
        php7.1-mbstring \
        php7.1-mysql \
        php7.1-pdo \
        php7.1-xdebug \
        php7.1-xml \
        php7.1-zip \
        php-apcu \
        supervisor \
        wget && \

    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \

    cp /usr/share/zoneinfo/Europe/Paris /etc/localtime && echo "Europe/Paris" > /etc/timezone && \

    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \

    mkdir /run/php && \

    curl -L -o /tmp/nodejs.tar.gz https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz && \
    tar xfvz /tmp/nodejs.tar.gz -C /usr/local --strip-components=1 && \
    rm -f /tmp/nodejs.tar.gz && \
    npm install yarn -g

# Blackfire
RUN wget -O - https://packagecloud.io/gpg.key | apt-key add - \
    && echo "deb http://packages.blackfire.io/debian any main" | tee /etc/apt/sources.list.d/blackfire.list \
    && apt-get update \
    && apt-get install blackfire-agent \
    && version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") \
    && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/linux/amd64/$version \
    && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp \
    && mv /tmp/blackfire-*.so $(php -r "echo ini_get('extension_dir');")/blackfire.so \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > /etc/php/7.1/cli/conf.d/blackfire.ini \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > /etc/php/7.1/fpm/conf.d/blackfire.ini

COPY php.ini /etc/php/7.1/cli/conf.d/50-setting.ini
COPY php.ini /etc/php/7.1/fpm/conf.d/50-setting.ini
COPY pool.conf /etc/php/7.1/fpm/pool.d/www.conf
COPY nginx.conf /etc/nginx/nginx.conf
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
