FROM 1maa/alpine:3.9

ENV PHP_VERSION=7.3.6
ENV XHPROF_VERSION=5.0-beta3

ENV CFLAGS="-O3 -march=x86-64"
ENV LDFLAGS="-s"

RUN apk add --no-cache \
    # generic dependencies for compiling the source
    autoconf \
    automake \
    build-base \
    file \
    flex \
    git \
    libtool \
    re2c \

    upx \

    # dependencies for building specific extensions
    argon2-dev \
    bzip2-dev \
    curl-dev \
    gd-dev \
    gmp-dev \
    gpgme-dev \
    libevent-dev \
    libpng-dev \
    librdkafka-dev \
    openssl-dev \
    libsodium-dev \
    libxml2-dev \
    libxslt-dev \
    libzip-dev \
    postgresql-dev \
    readline-dev \
 && cd /tmp \
 && wget -q https://www.php.net/distributions/php-${PHP_VERSION}.tar.gz \
 && tar zxof php-${PHP_VERSION}.tar.gz \
 && cd ./php-${PHP_VERSION} \
 && ./configure \
    EXTENSION_DIR="/usr/lib/php" \

    --prefix=/usr \
    --sysconfdir=/etc/php/cli \
    --with-config-file-path=/etc/php/cli \
    --with-config-file-scan-dir=/etc/php/cli/conf.d \

    --enable-cli \
    --disable-fpm \
    --disable-cgi \
    --disable-phpdbg \

    --enable-bcmath \
    --enable-calendar \
    --enable-exif \
    --enable-ftp \
    --enable-mbstring \
    --enable-opcache \
    --enable-pcntl \
    --enable-shmop \
    --enable-soap \
    --enable-sockets \
    --enable-wddx \
    --enable-zip \

    --with-bz2 \
    --with-curl \
    --with-gd \
    --with-gmp \
    --with-libzip \
    --with-mysqli \
    --with-openssl \
    --with-password-argon2 \
    --with-pdo-mysql \
    --with-pdo-pgsql \
    --with-pgsql \
    --with-readline \
    --with-sodium \
    --with-xmlrpc \
    --with-xsl \
    --with-zlib \
 && make -j$(nproc) \
 && make install \
 && wget -q https://github.com/tideways/php-xhprof-extension/archive/v${XHPROF_VERSION}.tar.gz \
 && tar zxof v${XHPROF_VERSION}.tar.gz \
 && cd ./php-xhprof-extension-${XHPROF_VERSION} \
 && phpize \
 && ./configure \
 && make \
 && make install \
 && chmod +x /usr/lib/php/*.so \
 && pecl install \
    apcu \
    ds \
    event \
    gnupg \
    igbinary \
    mongodb \
    msgpack \
    pcov \
    protobuf \
    rdkafka \
    redis \
    xdebug \
 && chmod +x /usr/lib/php/*.so \
 && cd /tmp \
 && rm -rf php-${PHP_VERSION} \
 && tar zxof php-${PHP_VERSION}.tar.gz \
 && cd ./php-${PHP_VERSION} \
 && ./configure \
    EXTENSION_DIR="/usr/lib/php" \

    --prefix=/usr \
    --sysconfdir=/etc/php/fpm \
    --with-config-file-path=/etc/php/fpm \
    --with-config-file-scan-dir=/etc/php/fpm/conf.d \

    --enable-fpm \
    --disable-cli \
    --disable-cgi \
    --disable-phpdbg \

    --enable-bcmath \
    --enable-calendar \
    --enable-exif \
    --enable-ftp \
    --enable-mbstring \
    --enable-opcache \
    --enable-shmop \
    --enable-soap \
    --enable-sockets \
    --enable-wddx \
    --enable-zip \

    --with-bz2 \
    --with-curl \
    --with-gd \
    --with-gmp \
    --with-libzip \
    --with-mysqli \
    --with-openssl \
    --with-password-argon2 \
    --with-pdo-mysql \
    --with-pdo-pgsql \
    --with-pgsql \
    --with-sodium \
    --with-xmlrpc \
    --with-xsl \
    --with-zlib \
 && make -j$(nproc) \
 && mv sapi/fpm/php-fpm /usr/sbin/php-fpm \
 && cp php.ini-production /tmp/php.ini \

 && sed -e 's#@php_fpm_sysconfdir@#/etc/php/fpm#' \
        -e 's#;daemonize = yes#daemonize = no#' \
        -e 's#;error_log = log/php-fpm.log#error_log = /dev/stderr#' \
        sapi/fpm/php-fpm.conf.in > /tmp/php-fpm.conf \

 && sed -e 's#user = @php_fpm_user@#user = root#' \
        -e 's#group = @php_fpm_group@#group = root#' \
        -e 's#listen = 127.0.0.1:9000#listen = 9000#' \
        sapi/fpm/www.conf.in > /tmp/www.conf \

 && upx /usr/bin/php /usr/sbin/php-fpm


FROM 1maa/alpine:3.9

COPY etc /etc/php

COPY --from=0 /tmp/php.ini      /etc/php/cli/php.ini
COPY --from=0 /tmp/php.ini      /etc/php/fpm/php.ini
COPY --from=0 /tmp/php-fpm.conf /etc/php/fpm/php-fpm.conf
COPY --from=0 /tmp/www.conf     /etc/php/fpm/php-fpm.d/www.conf
COPY --from=0 /usr/bin/php      /usr/bin/php
COPY --from=0 /usr/sbin/php-fpm /usr/sbin/php-fpm
COPY --from=0 /usr/lib/php/*.so /usr/lib/php/

RUN apk add --no-cache \
    argon2-libs \
    gmp \
    gpgme \
    libcurl \
    libevent \
    libgd \
    libpq \
    librdkafka \
    libsodium \
    libxml2 \
    libxslt \
    libzip \
    readline \
 && rm /etc/php/mods-available/libsodium.ini

EXPOSE 9000

CMD ["/usr/sbin/php-fpm", "-R"]
