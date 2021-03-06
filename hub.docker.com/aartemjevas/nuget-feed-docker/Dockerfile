FROM nginx

ENV APP_BASE /var/www
ENV APP_BRANCH master
ENV DEBIAN_VERSION jessie
# ENV HHVM_VERSION 3.9.1~$DEBIAN_VERSION

# Install HHVM, Supervisor and PHP DBO connectors
RUN apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x5a16e7281be7a449 && \
    echo deb http://dl.hhvm.com/debian $DEBIAN_VERSION main | tee /etc/apt/sources.list.d/hhvm.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends hhvm \
      php5-mysql php5-sqlite \
      supervisor

# Copy in the project
RUN rm -rf $APP_BASE
COPY simple-nuget-server $APP_BASE
RUN rm -rf $APP_BASE/.git && \
  chown www-data:www-data $APP_BASE/db $APP_BASE/packagefiles && \
  chown 0770 $APP_BASE/db $APP_BASE/packagefiles

# Activate the nginx configuration
RUN rm /etc/nginx/conf.d/default*.conf
COPY config/nuget.conf /etc/nginx/conf.d/

# Configure file sizes
RUN echo "post_max_size = 2048M" >> /etc/hhvm/php.ini && \
    echo "upload_max_filesize = 2048M" >> /etc/hhvm/php.ini && \
	echo "max_execution_time = 600000" >> /etc/hhvm/php.ini && \
	echo "hhvm.server.upload.upload_max_file_size = 2048M" >> /etc/hhvm/server.ini && \
	echo "hhvm.server.max_post_size = 2048M" >> /etc/hhvm/server.ini && \
    perl -pi -e 's/^(\s*)(root.+?;)/\1\2\n\1client_max_body_size 2048M;/' /etc/nginx/conf.d/nuget.conf

# Install the supervisor configuration
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY scripts/*.sh /tmp/
RUN chmod +x /tmp/*.sh

# The base URL
ENV BASE_URL /

# Set randomly generated API key
RUN echo $(date +%s | sha256sum | base64 | head -c 32; echo) > $APP_BASE/.api-key && \
    echo "Auto-Generated NuGet API key: $(cat $APP_BASE/.api-key)" && \
    sed -i $APP_BASE/inc/config.php -e "s/ChangeThisKey/$(cat $APP_BASE/.api-key)/"

# Define the volumes
VOLUME ["$APP_BASE/db", "$APP_BASE/packagefiles"]

# Fire in the hole!
CMD ["supervisord", "-n"]
