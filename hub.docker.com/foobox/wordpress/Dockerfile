FROM foobox/php:7.1-cli

ARG WORDPRESS_VERSION=4.9.8

RUN wget https://wordpress.org/wordpress-$WORDPRESS_VERSION.tar.gz && \
	tar -xvf wordpress-$WORDPRESS_VERSION.tar.gz && \
	rm wordpress-$WORDPRESS_VERSION.tar.gz && \
	mv wordpress/* htdocs && \
	rmdir wordpress

COPY . /var/www/html
RUN composer-extension-install

WORKDIR /var/www/html/htdocs
CMD ["php", "/var/www/html/htdocs/wp-cron.php"]
