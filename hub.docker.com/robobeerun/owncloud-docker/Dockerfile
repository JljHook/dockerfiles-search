FROM owncloud:10.0.3-fpm
LABEL maintainer "Erwin Mueller <erwin.mueller@deventm.com>"

ARG APT_CACHE

RUN set -x \
  # Optional add proxy entries for apt.
  && if [ -n "${APT_CACHE}" ]; then \
  echo Acquire::ftp::Proxy \"$APT_CACHE\"; >> /etc/apt/apt.conf.d/08proxy;\
  echo Acquire::http::Proxy \"$APT_CACHE\"; >> /etc/apt/apt.conf.d/08proxy;\
  echo Acquire::https::Proxy \"$APT_CACHE\"; >> /etc/apt/apt.conf.d/08proxy;\
  fi

RUN set -x \
  && docker-php-ext-install ctype \
  && docker-php-ext-install pcntl

RUN set -x \
  && DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install -y \
    rsync \
  && rm -rf /var/lib/apt/lists/*

ENV DEBUG="false" \
  WEB_ROOT="/var/www/html" \
  WEB_USER="www-data" \
  PHP_MAX_EXECUTION_TIME="300" \
  PHP_MEMORY_LIMIT="128M" \
  PHP_FPM_MAX_CHILDREN="10" \
  PHP_FPM_START_SERVERS="2" \
  PHP_FPM_MIN_SPARE_SERVERS="2" \
  PHP_FPM_MAX_SPARE_SERVERS="5" \
  PHP_FPM_MAX_REQUESTS="500" \
  PHP_FPM_REQUEST_SLOWLOG_TIMEOUT="30s" \
  PHP_FPM_CATCH_WORKERS_OUTPUT="1" \
  PHP_OPCACHE_ENABLE_CLI="1" \
  PHP_OPCACHE_ENABLE="1" \
  PHP_OPCACHE_MEMORY_CONSUMPTION="128" \
  PHP_OPCACHE_MAX_ACCELERATED_FILES="6000"

ADD rootfs/ /

RUN set -x \
  && chmod +x /*.sh \
  && mkdir -p /data \
  && ln -sf /dev/stdout /var/log/php-fpm-slow.log \
  && mv /usr/local/bin/docker-entrypoint.sh /usr/local/bin/docker-entrypoint-old.sh

VOLUME ["/data"]

USER www-data

WORKDIR ${WEB_ROOT}

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["php-fpm"]
