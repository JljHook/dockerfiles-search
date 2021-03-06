FROM alpine:3.6
MAINTAINER Stalin Pereira, letter2stalin@gmail.com

ENV KONG_VERSION 0.13.0
ENV KONG_SHA256 93a277a98276cc857198db66cb8257dc30009bcda2ae0a3322f6e20a6cf8d91a

RUN apk add --no-cache --virtual .build-deps wget tar ca-certificates \
	&& apk add --no-cache libgcc openssl pcre perl tzdata \
	&& wget -O kong.tar.gz "https://bintray.com/kong/kong-community-edition-alpine-tar/download_file?file_path=kong-community-edition-$KONG_VERSION.apk.tar.gz" \
	&& echo "$KONG_SHA256 *kong.tar.gz" | sha256sum -c - \
	&& tar -xzf kong.tar.gz -C /tmp \
	&& rm -f kong.tar.gz \
	&& cp -R /tmp/usr / \
	&& rm -rf /tmp/usr \
	&& cp -R /tmp/etc / \
	&& rm -rf /tmp/etc \
	&& wget -O kong-plugin-response-cache.zip "https://github.com/wshirey/kong-plugin-response-cache/archive/master.zip" \
	&& mkdir -p /tmp/ \
	&& unzip kong-plugin-response-cache.zip -d /tmp/ \
	&& cd /tmp/kong-plugin-response-cache-master \
	&& luarocks make --verbose \
	&& apk del .build-deps

COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 8000 8443 8001 8444

STOPSIGNAL SIGTERM

CMD ["/usr/local/openresty/nginx/sbin/nginx", "-c", "/usr/local/kong/nginx.conf", "-p", "/usr/local/kong/"]
