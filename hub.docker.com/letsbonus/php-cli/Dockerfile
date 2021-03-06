FROM alpine:edge

RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
 && apk add --no-cache \
    git \
    openssh \
    php5-apcu \
    php5-bcmath \
    php5-bz2 \
    php5-cli \
    php5-ctype \
    php5-curl \
    php5-exif \
    php5-gd \
    php5-iconv \
    php5-intl \
    php5-json \
    php5-libsodium \
    php5-opcache \
    php5-openssl \
    php5-pcntl \
    php5-pdo_mysql \
    php5-pdo_pgsql \
    php5-pdo_sqlite \
    php5-phar \
    php5-posix \
    php5-redis \
    php5-soap \
    php5-xdebug \
    php5-xml \
    php5-xsl \
    php5-zip \
    php5-zlib \
    tini \
 && sed -i 's@expose_php = On@expose_php = Off@' /etc/php5/php.ini \
 && echo 'root:lbmonkey' | chpasswd \
 && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

COPY dev/profile.56 /etc/profile
COPY dev/conf.d/*   /etc/php5/conf.d/
COPY dev/ssh/*      /etc/ssh/
COPY dev/tooling/*  /usr/local/bin/

RUN chmod 644 /etc/ssh/* \
 && chmod 600 /etc/ssh/ssh_host_dsa_key \
 && chmod 600 /etc/ssh/ssh_host_ecdsa_key \
 && chmod 600 /etc/ssh/ssh_host_ed25519_key \
 && chmod 600 /etc/ssh/ssh_host_rsa_key

EXPOSE 22

ENTRYPOINT ["/sbin/tini", "--"]

CMD ["/usr/sbin/sshd", "-D"]
