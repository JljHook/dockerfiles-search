FROM debian:jessie

ENV DEBIAN_FRONTEND noninteractive

# Define versions
ENV OPENRESTY_VERSION 1.11.2.1
ENV PAGESPEED_VERSION 1.11.33.4-beta
ENV PAGESPEED_PSOL_VERSION 1.11.33.4
ENV OPENSSL_VERSION 1.0.2j
ENV PCRE_VERSION 8.39
ENV ZLIB_VERSION 1.2.8

# Default environment
# Can be overridden at runtime using -e ENVIRONMENT=...
ENV ENVIRONMENT development

RUN apt-get update \
    && apt-get install -y build-essential wget ca-certificates

RUN (wget -qO - https://github.com/pagespeed/ngx_pagespeed/archive/v${PAGESPEED_VERSION}.tar.gz | tar zxf - -C /tmp)
RUN (wget --no-check-certificate -qO - https://dl.google.com/dl/page-speed/psol/${PAGESPEED_PSOL_VERSION}.tar.gz | tar zxf - -C /tmp/ngx_pagespeed-${PAGESPEED_VERSION}/)
RUN (wget -qO - http://openresty.org/download/openresty-${OPENRESTY_VERSION}.tar.gz | tar zxf - -C /tmp)
RUN (wget -qO - https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz | tar zxf - -C /tmp)
RUN (wget -qO - ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-${PCRE_VERSION}.tar.gz | tar zxf - -C /tmp)
RUN (wget -qO - http://zlib.net/zlib-${ZLIB_VERSION}.tar.gz | tar zxf - -C /tmp)

WORKDIR /tmp/openresty-${OPENRESTY_VERSION}
RUN ./configure --prefix=/usr/share/nginx \
        --user=www-data \
        --group=www-data \
        --with-luajit \
        --sbin-path=/usr/sbin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --http-client-body-temp-path=/var/lib/nginx/body \
        --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
        --http-log-path=/var/log/nginx/access.log \
        --http-proxy-temp-path=/var/lib/nginx/proxy \
        --lock-path=/var/lock/nginx.lock \
        --pid-path=/var/run/nginx.pid \
        --with-ipv6 \
        --with-http_ssl_module \
        # --with-http_spdy_module \
        --with-pcre=/tmp/pcre-${PCRE_VERSION} \
        --with-zlib=/tmp/zlib-${ZLIB_VERSION} \
        --with-openssl=/tmp/openssl-${OPENSSL_VERSION} \
        --with-md5=/tmp/openssl-${OPENSSL_VERSION} \
        --with-md5-asm \
        --with-sha1=/tmp/openssl-${OPENSSL_VERSION} \
        --with-sha1-asm \
        --with-pcre-jit \
        --with-http_stub_status_module \
        --with-http_secure_link_module \
        --with-http_gzip_static_module \
        --with-http_gunzip_module \
        --without-http_uwsgi_module \
        --without-http_scgi_module \
        --without-http_redis_module \
        --without-http_memc_module \
        --without-http_memcached_module \
        --without-http_coolkit_module \
        --without-http_form_input_module \
        --without-http_rds_json_module \
        --without-http_rds_csv_module \
        --without-http_empty_gif_module \
        --without-http_browser_module \
        --without-http_userid_module \
        --without-http_autoindex_module \
        --without-http_geo_module \
        --without-http_split_clients_module \
        --without-mail_pop3_module \
        --without-mail_imap_module \
        --without-mail_smtp_module \
        --without-http_encrypted_session_module \
        --without-lua_resty_memcached \
        --add-module=/tmp/ngx_pagespeed-${PAGESPEED_VERSION}

RUN make && make install
WORKDIR /

# Cleanup
RUN rm -Rf /tmp/* \
    && apt-get purge -yqq build-essential wget ca-certificates \
    && apt-get autoremove -yqq \
    && apt-get clean all

# Create folders required by nginx & set proper permissions
RUN mkdir /var/lib/nginx \
    && chown -R www-data:www-data /var/lib/nginx \
    && mkdir /var/lib/nginx/proxy \
    && mkdir /var/lib/nginx/body \
    && mkdir /var/lib/nginx/fastcgi \
    && chmod 777 /var/log/nginx

# Add full write permissions to the pagespeed cache folder
RUN mkdir /var/ngx_pagespeed_cache \
    && chmod 777 /var/ngx_pagespeed_cache
	
# Link stdout / stderr
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

# Copy our custom configuration
ADD nginx /etc/nginx/

# Define mountable directories.
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf", "/var/log/nginx"]

# Define working directory.
WORKDIR /etc/nginx

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]