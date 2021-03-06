FROM alpine:3.5

MAINTAINER Stas Alekseev "stas.alekseev@gmail.com"

ENV NGINX_VERSION 1.10.3
ENV NGINX_KAFKA_MODULE_VERSION 0.9.1
ENV NGINX_GRAPHITE_MODULE_VERSION 1.1.0
ENV NGINX_LDAP_MODULE_VERSION dbcef31bebb2d54b6120422d0b178bbf78bc48f7
ENV LIBRDKAFKA_VER 0.9.3

RUN GPG_KEYS=B0F4253373F8F6F510D42178520A9993A1C052F8 \
	&& CONFIG="\
		--prefix=/etc/nginx \
		--sbin-path=/usr/sbin/nginx \
		--modules-path=/usr/lib/nginx/modules \
		--conf-path=/etc/nginx/nginx.conf \
		--error-log-path=/var/log/nginx/error.log \
		--http-log-path=/var/log/nginx/access.log \
		--pid-path=/var/run/nginx.pid \
		--lock-path=/var/run/nginx.lock \
		--http-client-body-temp-path=/var/cache/nginx/client_temp \
		--http-proxy-temp-path=/var/cache/nginx/proxy_temp \
		--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
		--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
		--http-scgi-temp-path=/var/cache/nginx/scgi_temp \
		--user=nginx \
		--group=nginx \
		--with-http_ssl_module \
		--with-http_realip_module \
		--with-http_addition_module \
		--with-http_sub_module \
		--with-http_dav_module \
		--with-http_flv_module \
		--with-http_mp4_module \
		--with-http_gunzip_module \
		--with-http_gzip_static_module \
		--with-http_random_index_module \
		--with-http_secure_link_module \
		--with-http_stub_status_module \
		--with-http_auth_request_module \
		--with-http_xslt_module=dynamic \
		--with-http_image_filter_module=dynamic \
		--with-http_geoip_module=dynamic \
		--with-http_perl_module=dynamic \
		--add-dynamic-module=/usr/src/ngx_kafka_module-$NGINX_KAFKA_MODULE_VERSION \
		--add-dynamic-module=/usr/src/graphite-nginx-module-$NGINX_GRAPHITE_MODULE_VERSION \
		--add-dynamic-module=/usr/src/nginx-auth-ldap-$NGINX_LDAP_MODULE_VERSION \
		--with-threads \
		--with-stream \
		--with-stream_ssl_module \
		--with-http_slice_module \
		--with-mail \
		--with-mail_ssl_module \
		--with-file-aio \
		--with-http_v2_module \
		--with-ipv6 \
	" \
	&& addgroup -S nginx \
	&& adduser -D -S -h /var/cache/nginx -s /sbin/nologin -G nginx nginx \
	&& apk add --no-cache --virtual .build-deps \
	    bash \
	    python \
		gcc \
		g++ \
		libc-dev \
		make \
		libressl-dev \
		pcre-dev \
		zlib-dev \
		linux-headers \
		curl \
		gnupg \
		libxslt-dev \
		gd-dev \
		geoip-dev \
		perl-dev \
		openldap-dev \
	&& curl -fSL https://github.com/edenhill/librdkafka/archive/v$LIBRDKAFKA_VER.tar.gz -o librdkafka-$LIBRDKAFKA_VER.tar.gz \
	&& curl -fSL http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz -o nginx.tar.gz \
	&& curl -fSL http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz.asc  -o nginx.tar.gz.asc \
	&& curl -fSL https://github.com/brg-liuwei/ngx_kafka_module/archive/v$NGINX_KAFKA_MODULE_VERSION.tar.gz -o ngx_kafka_module-$NGINX_KAFKA_MODULE_VERSION.tar.gz \
	&& curl -fSL https://github.com/mailru/graphite-nginx-module/archive/v$NGINX_GRAPHITE_MODULE_VERSION.tar.gz -o graphite-nginx-module-$NGINX_GRAPHITE_MODULE_VERSION.tar.gz \
	&& curl -fSL https://github.com/kvspb/nginx-auth-ldap/archive/$NGINX_LDAP_MODULE_VERSION.tar.gz -o nginx-auth-ldap-$NGINX_LDAP_MODULE_VERSION.tar.gz \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEYS" \
	&& gpg --batch --verify nginx.tar.gz.asc nginx.tar.gz \
	&& rm -r "$GNUPGHOME" nginx.tar.gz.asc \
	&& mkdir -p /usr/src \
	&& tar -zxC /usr/src -f librdkafka-$LIBRDKAFKA_VER.tar.gz \
	&& tar -zxC /usr/src -f nginx.tar.gz \
	&& tar -zxC /usr/src -f ngx_kafka_module-$NGINX_KAFKA_MODULE_VERSION.tar.gz \
	&& tar -zxC /usr/src -f graphite-nginx-module-$NGINX_GRAPHITE_MODULE_VERSION.tar.gz \
	&& tar -zxC /usr/src -f nginx-auth-ldap-$NGINX_LDAP_MODULE_VERSION.tar.gz \
	&& rm librdkafka-$LIBRDKAFKA_VER.tar.gz \
	&& rm ngx_kafka_module-$NGINX_KAFKA_MODULE_VERSION.tar.gz \
	&& rm graphite-nginx-module-$NGINX_GRAPHITE_MODULE_VERSION.tar.gz \
	&& rm nginx-auth-ldap-$NGINX_LDAP_MODULE_VERSION.tar.gz \
	&& rm nginx.tar.gz \
	&& cd /usr/src/librdkafka-$LIBRDKAFKA_VER \
	&& ./configure --prefix=/usr \
	&& make -j "$(getconf _NPROCESSORS_ONLN)" \
	&& make install \
	&& cd /usr/src/nginx-$NGINX_VERSION \
	&& ./configure $CONFIG --with-debug \
	&& make -j$(getconf _NPROCESSORS_ONLN) \
	&& mv objs/nginx objs/nginx-debug \
	&& mv objs/ngx_http_xslt_filter_module.so objs/ngx_http_xslt_filter_module-debug.so \
	&& mv objs/ngx_http_image_filter_module.so objs/ngx_http_image_filter_module-debug.so \
	&& mv objs/ngx_http_geoip_module.so objs/ngx_http_geoip_module-debug.so \
	&& mv objs/ngx_http_perl_module.so objs/ngx_http_perl_module-debug.so \
	&& mv objs/ngx_http_kafka_module.so objs/ngx_http_kafka_module-debug.so \
	&& mv objs/ngx_http_graphite_module.so objs/ngx_http_graphite_module-debug.so \
	&& mv objs/ngx_http_auth_ldap_module.so objs/ngx_http_auth_ldap_module-debug.so \
	&& ./configure $CONFIG \
	&& make -j$(getconf _NPROCESSORS_ONLN) \
	&& make install \
	&& rm -rf /etc/nginx/html/ \
	&& mkdir /etc/nginx/conf.d/ \
	&& mkdir -p /usr/share/nginx/html/ \
	&& install -m644 html/index.html /usr/share/nginx/html/ \
	&& install -m644 html/50x.html /usr/share/nginx/html/ \
	&& install -m755 objs/nginx-debug /usr/sbin/nginx-debug \
	&& install -m755 objs/ngx_http_xslt_filter_module-debug.so /usr/lib/nginx/modules/ngx_http_xslt_filter_module-debug.so \
	&& install -m755 objs/ngx_http_image_filter_module-debug.so /usr/lib/nginx/modules/ngx_http_image_filter_module-debug.so \
	&& install -m755 objs/ngx_http_geoip_module-debug.so /usr/lib/nginx/modules/ngx_http_geoip_module-debug.so \
	&& install -m755 objs/ngx_http_perl_module-debug.so /usr/lib/nginx/modules/ngx_http_perl_module-debug.so \
	&& install -m755 objs/ngx_http_kafka_module-debug.so /usr/lib/nginx/modules/ngx_http_kafka_module-debug.so \
	&& install -m755 objs/ngx_http_graphite_module-debug.so /usr/lib/nginx/modules/ngx_http_graphite_module-debug.so \
	&& install -m755 objs/ngx_http_auth_ldap_module-debug.so /usr/lib/nginx/modules/ngx_http_auth_ldap_module-debug.so \
	&& ln -s ../../usr/lib/nginx/modules /etc/nginx/modules \
	&& strip /usr/sbin/nginx* \
	&& strip /usr/lib/nginx/modules/*.so \
	&& rm -rf /usr/src/nginx-$NGINX_VERSION \
	&& rm -rf /usr/src/ngx_kafka_module-$NGINX_KAFKA_MODULE_VERSION \
	&& rm -rf /usr/src/graphite-nginx-module-$NGINX_GRAPHITE_MODULE_VERSION \
	&& rm -rf /usr/src/nginx-auth-ldap-$NGINX_LDAP_MODULE_VERSION \
	\
	# Bring in gettext so we can get `envsubst`, then throw
	# the rest away. To do this, we need to install `gettext`
	# then move `envsubst` out of the way so `gettext` can
	# be deleted completely, then move `envsubst` back.
	&& apk add --no-cache --virtual .gettext gettext \
	&& mv /usr/bin/envsubst /tmp/ \
	\
	&& runDeps="$( \
		scanelf --needed --nobanner /usr/sbin/nginx /usr/lib/nginx/modules/*.so /tmp/envsubst \
			| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
			| sort -u \
			| xargs -r apk info --installed \
			| sort -u \
	)" \
	&& apk add --no-cache --virtual .nginx-rundeps $runDeps \
	&& apk del .build-deps \
	&& apk del .gettext \
	&& mv /tmp/envsubst /usr/local/bin/ \
	\
	# forward request and error logs to docker log collector
	&& ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

COPY nginx.conf /etc/nginx/nginx.conf
COPY nginx.vh.default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
