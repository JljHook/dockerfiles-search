FROM 1maa/php-cli:7.0

RUN apk add --no-cache \
    php7-fpm \
 && sed -i 's@;clear_env = no@clear_env = no@'        /etc/php7/php-fpm.d/www.conf \
 && sed -i 's@listen = 127.0.0.1:9000@listen = 9000@' /etc/php7/php-fpm.d/www.conf

EXPOSE 9000

CMD ["/usr/sbin/php-fpm7", "--nodaemonize"]
