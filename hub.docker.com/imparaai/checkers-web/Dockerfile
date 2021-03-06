FROM alpine:edge

# Install packages
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/edge/testing' >> /etc/apk/repositories && \
  apk update && \
  apk upgrade && \
  apk --no-cache add \
    bash \
    gawk \
    sed \
    grep \
    bc \
    coreutils \
    vim \
    supervisor \
    curl \
    wget \
    openssh \
    git \
    zip \
    bzip2 \
    libressl-dev \
    php7 \
    php7-bcmath \
    php7-curl \
    php7-ctype \
    php7-dom \
    php7-fpm \
    php7-gd \
    php7-iconv \
    php7-json \
    php7-mcrypt \
    php7-mbstring \
    php7-mysqlnd \
    php7-pdo \
    php7-pdo_mysql \
    php7-posix \
    php7-phar \
    php7-openssl \
    php7-session \
    php7-soap \
    php7-tokenizer \
    php7-xml \
    php7-xmlwriter \
    php7-zip \
    php7-zlib \
    npm \
    yarn

# Get Composer
RUN curl -sS https://getcomposer.org/installer | php7 -- --install-dir=/usr/local/bin --filename=composer

# Add configuration files
COPY docker/conf/supervisord.conf /etc/supervisor.d/supervisord.ini
COPY docker/conf/php.ini /etc/php7/conf.d/40-custom.ini
COPY docker/conf/php-fpm-www.conf /etc/php7/php-fpm.d/www.conf
COPY docker/conf/.bashrc /root/.bashrc
COPY docker/conf/nginx.conf /etc/nginx/conf.d/default.conf
COPY docker/start.sh /bin/original_start.sh

# Set up bash, php cli, fpm conf and pid file, start script
RUN ln -snf /bin/bash /bin/sh && \
    sed -i -e 's/\r$//' /root/.bashrc && \
    tr -d '\r' < /bin/original_start.sh > /bin/start.sh && \
    chmod -R 700 /bin/start.sh && \
    printf "color desert" > /root/.vimrc && \
    addgroup -g 82 -S www-data && \
    adduser -u 82 -D -S -G www-data www-data && \
    php-fpm7 && \
    rm -fr /var/cache/apk/* && \
    mkdir /root/.ssh && \
    touch /root/.ssh/known_hosts && \
    ssh-keyscan github.com >> /root/.ssh/known_hosts

# Install nginx after www-data has been created
RUN apk --no-cache add nginx && \
    mkdir -p /run/nginx

# Install app
COPY . /var/www/web
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN cd /var/www/web && \
    composer install -n --no-dev && \
    chown -R www-data:www-data /var/www/web/storage && \
    npm install -g npm && \
    yarn install

WORKDIR /var/www/web

EXPOSE 9000

ENV TERM xterm-color

CMD ["sh", "/bin/start.sh"]