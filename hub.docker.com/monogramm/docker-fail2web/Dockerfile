FROM nginx:alpine

LABEL maintainer="Mathieu BRUNOT <mathieu.brunot at monogramm dot io>"

ENV FAIL2REST_ADDR=http://localhost:5000/ \
	FAIL2REST_USER=admin.fail2ban \
	FAIL2REST_PASSWD=youshouldoverwritethis \
	FAIL2REST_PASSWD_COST=15

COPY docker-entrypoint.sh /entrypoint.sh
COPY nginx.vh.default.conf /etc/nginx/conf.d/default.conf

# Get fail2web
ADD https://github.com/Sean-Der/fail2web/archive/master.tar.gz /tmp/fail2web.tar.gz

# Install the packages we need
# Make sure the entrypoint is executable
# Get and install fail2web
RUN set -ex; \
	apk add --no-cache \
		apache2-utils \
		tar \
	; \
	chmod 755 /entrypoint.sh; \
	mkdir -p /var/www/html/; \
	tar xzf /tmp/fail2web.tar.gz -C /var/www/html/; \
	mv /var/www/html/fail2web-*/ /var/www/html/fail2web/; \
	rm -f /tmp/fail2web.tar.gz

VOLUME /srv/fail2web/ /var/www/html/fail2web

ENTRYPOINT ["/entrypoint.sh"]
