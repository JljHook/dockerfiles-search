FROM alpine:edge

RUN set -x \
    && apk --no-cache add \    
           php7 \
           php7-dom \
           php7-curl \
           php7-ctype \
           php7-fpm \
           php7-gd \
           php7-iconv \
           php7-json \
           php7-mbstring \
           php7-mysqli \
           php7-session \
           php7-simplexml \
           php7-xml \
           php7-xmlreader \
           php7-xmlwriter \
           php7-zip \
    && apk --no-cache \
           --allow-untrusted \
           --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ \
       add php7-libsodium \
    && rm -rf /var/cache/apk/*

COPY rootfs /

EXPOSE 9000

CMD ["php-fpm7", "-F"]
