FROM php:fpm-alpine

RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev zlib-dev && \
    # config gd
    docker-php-ext-configure gd \
     --with-gd \
     --with-freetype-dir=/usr/include/ \
     --with-png-dir=/usr/include/ \
     --with-jpeg-dir=/usr/include/ && \
    # get cpu core count , -c is grep count line as begin with processor line
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    # install gd
    docker-php-ext-install -j${NPROC} gd && \
    # wtf ?
    apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev && \
    # install pdo
    docker-php-ext-install pdo_mysql && \
    apk add --update autoconf alpine-sdk libpq openssl-dev libssl1.0 libstdc++ && \
    yes | pecl install swoole && \
    apk del --no-cache openssl-dev alpine-sdk && \
    # install zip for excel export 
    docker-php-ext-install zip && \
    # install bcmath for  Arbitrary Precision Mathematics
    docker-php-ext-install bcmath 

COPY ./swoole.ini /usr/local/etc/php/conf.d/swoole.ini
