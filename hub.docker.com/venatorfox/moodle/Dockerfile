FROM centos:7
LABEL maintainer="Adam W Zheng <adam.w.zheng@icloud.com>"

ENV S6_RELEASE 1.22.1.0
ENV MOODLE_RELEASE MOODLE_37_STABLE

#Add s6-overlay
ADD https://github.com/just-containers/s6-overlay/releases/download/v$S6_RELEASE/s6-overlay-amd64.tar.gz /tmp/
RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C / --exclude="./bin" \
 && tar xzf /tmp/s6-overlay-amd64.tar.gz -C /usr ./bin

#Install moodle requirements
RUN yum -y install centos-release-scl \
 && yum -y install rh-nginx114 rh-php72 rh-php72-php-fpm rh-php72-php-xml rh-php72-php-pgsql rh-php72-php-mysqlnd rh-php72-php-curl rh-php72-php-zip rh-php72-php-gd rh-php72-php-mbstring rh-php72-php-xmlrpc rh-php72-php-soap rh-php72-php-intl sclo-php72-php-pecl-memcached git cronie \
 && sed -i 's/user = apache/user = nginx/' /etc/opt/rh/rh-php72/php-fpm.d/www.conf \
 && sed -i 's/group = apache/group = nginx/' /etc/opt/rh/rh-php72/php-fpm.d/www.conf

#Clone Moodle
RUN rm -rfv /opt/rh/rh-nginx114/root/usr/share/nginx/html/* \
 && git clone --depth=1 -b $MOODLE_RELEASE git://git.moodle.org/moodle.git /opt/rh/rh-nginx114/root/usr/share/nginx/html/ \
 && mkdir -p /var/moodledata

#Configure webserver as per Moodle Docs
RUN sed -i '/        location \/ {/ a \\t\tindex  index.php index.html index.htm;' /etc/opt/rh/rh-nginx114/nginx/nginx.conf \
 && sed -i '/pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000/ a\\tlocation ~ [^\/]\\.php(\/|\$) {\n\t\tfastcgi_split_path_info ^(.+\\.php)(\/.+)\$;\n\t\tfastcgi_index\tindex.php;\n\t\tfastcgi_pass\t127.0.0.1:9000;\n\t\tinclude\tfastcgi_params;\n\t\tfastcgi_param\tPATH_INFO\t\$fastcgi_path_info;\n\t\tfastcgi_param\tSCRIPT_FILENAME\t\$document_root\$fastcgi_script_name;\n\t}' /etc/opt/rh/rh-nginx114/nginx/nginx.conf

#Redirect nginx logs
RUN ln -sf /dev/stdout /var/opt/rh/rh-nginx114/log/nginx/access.log \
 && ln -sf /dev/stderr /var/opt/rh/rh-nginx114/log/nginx/error.log

#Add service nginx to s6-supervisor
RUN mkdir -p /etc/services.d/nginx/ \
 && touch /etc/services.d/nginx/run \
 && echo '#!/usr/bin/execlineb -P' > /etc/services.d/nginx/run \
 && echo '/opt/rh/rh-nginx114/root/usr/sbin/nginx -g "daemon off;"' >> /etc/services.d/nginx/run

#Add service php-fpm to s6-supervisor
RUN mkdir -p /etc/services.d/php-fpm/ \
 && touch /etc/services.d/php-fpm/run \
 && echo '#!/usr/bin/execlineb -P' > /etc/services.d/php-fpm/run \
 && echo '/opt/rh/rh-php72/root/usr/sbin/php-fpm' >> /etc/services.d/php-fpm/run --nodaemonize \
 && sed -i 's|pid = /run/php-fpm/php-fpm.pid|pid = /run/php-fpm.pid|g' /etc/opt/rh/rh-php72/php-fpm.conf

#Add service cron to s6-supervisor
RUN mkdir -p /etc/services.d/cron/ \
 && touch /etc/services.d/cron/run \
 && echo '#!/usr/bin/execlineb -P' > /etc/services.d/cron/run \
 && echo 'exec /usr/sbin/crond -n' >> /etc/services.d/cron/run

#Copy the Moodle CLI install script into the container to be executed on startup
COPY install-moodle.sh /etc/cont-init.d/
RUN chmod u+x /etc/cont-init.d/install-moodle.sh

RUN yum -y update && yum clean all && rm -rf /var/cache/yum && > /var/log/yum.log

ENTRYPOINT ["/init"]
