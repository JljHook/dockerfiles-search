FROM nguyenphuongthanhf/ubuntu1604:latest

# Add default apache2 user
RUN useradd -u 1000 -m --shell /bin/bash apache2 && \
    echo "apache2:P@ssw0rd123456" | chpasswd

ENV  APACHE2_CONFIG_FOLDER  /data/apache2/config
ENV  APACHE2_DATA_FOLDER    /data/apache2/www
ENV  APACHE2_LOG_FOLDER     /data/apache2/log
ENV  PHP5_CONFIG_FOLDER   /data/php/config
ENV  PHP5_LOG_FOLDER      /data/php/log
RUN DEBIAN_FRONTEND=noninteractive \
&& apt-get install -y python-software-properties \
&& add-apt-repository -y ppa:ondrej/php \
&& apt-get update -y \
&& apt-get install -y git apache2 libapache2-mod-xsendfile \
&& a2enmod rewrite \
&& a2enmod vhost_alias \
&& a2enmod xsendfile \
&& adduser www-data apache2 \
&& echo "ServerName localhost" >> /etc/apache2/apache2.conf \
&& mv /etc/apache2 /etc/apache2_origin \
&& mkdir -p $APACHE2_CONFIG_FOLDER \
&& rm $APACHE2_CONFIG_FOLDER -rf \
&& ln -s /etc/apache2_origin $APACHE2_CONFIG_FOLDER \
&& ln -s $APACHE2_CONFIG_FOLDER /etc/apache2 \
&& mv /var/www /var/www_origin \
&& mkdir -p $APACHE2_DATA_FOLDER \
&& rm $APACHE2_DATA_FOLDER -rf \
&& ln -s /var/www_origin $APACHE2_DATA_FOLDER \
&& ln -s $APACHE2_DATA_FOLDER /var/www \
&& chown apache2:apache2 -h /var/www \
&& rm /var/log/apache2 -R \
&& mkdir -p $APACHE2_LOG_FOLDER \
&& rm $APACHE2_LOG_FOLDER -rf \
&& ln -s $APACHE2_LOG_FOLDER /var/log/apache2 \
&& apt-get install -y libapache2-mod-php5.6 php5.6 php5.6-common php5.6-cli php5.6-dev php-pear php5.6-json php5.6-imagick php5.6-imap php5.6-intl php5.6-mcrypt php5.6-memcached php5.6-redis php5.6-mongo php5.6-xdebug php5.6-curl php5.6-mysqlnd php5.6-xcache php5.6-mcrypt php5.6-oauth php5.6-gd \
&& mv /etc/php /etc/php_origin \
&& mkdir -p $PHP5_CONFIG_FOLDER \
&& rm $PHP5_CONFIG_FOLDER  -rf \
&& ln -s /etc/php_origin $PHP5_CONFIG_FOLDER  \
&& ln -s $PHP5_CONFIG_FOLDER  /etc/php \
&& mkdir -p $PHP5_LOG_FOLDER \
&& rm $PHP5_LOG_FOLDER -rf \
&& ln -s $PHP5_LOG_FOLDER /var/log/php \
&& sed -e "s/^short_open_tag .*$/short_open_tag = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^asp_tags .*$/asp_tags = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^expose_php .*$/expose_php = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^display_errors .*$/display_errors = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^display_startup_errors .*$/display_startup_errors = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^log_errors .*$/log_errors = On/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^memory_limit .*$/memory_limit = 150M/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^allow_url_fopen .*$/allow_url_fopen = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^allow_url_include .*$/allow_url_include = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^\;date\.timezone .*$/date\.timezone = \"Asia\/Ho_Chi_Minh\"/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^safe_mode .*$/safe_mode = Off/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^disable_functions .*$/disable_functions = proc_open, popen, disk_free_space, diskfreespace, leak, system, shell_exec, escapeshellcmd, proc_nice, dl, symlink, show_source/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^max_execution_time .*$/max_execution_time = 60/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& export TMP=`echo "error_log\=${PHP5_LOG_FOLDER}/error.log" | sed -e "s/\//\\\\\\\\\//g"` \
&& sed -e "s/^\;error_log = php_errors\.log$/$TMP/g" -i /etc/php_origin/5.6/apache2/php.ini \
&& sed -e "s/^short_open_tag .*$/short_open_tag = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^asp_tags .*$/asp_tags = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^expose_php .*$/expose_php = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^display_errors .*$/display_errors = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^display_startup_errors .*$/display_startup_errors = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^log_errors .*$/log_errors = On/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^memory_limit .*$/memory_limit = 150M/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^allow_url_fopen .*$/allow_url_fopen = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^allow_url_include .*$/allow_url_include = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^\;date\.timezone .*$/date\.timezone = \"Asia\/Ho_Chi_Minh\"/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^safe_mode .*$/safe_mode = Off/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^disable_functions .*$/disable_functions = proc_open, popen, disk_free_space, diskfreespace, leak, system, shell_exec, escapeshellcmd, proc_nice, dl, symlink, show_source/g" -i /etc/php_origin/5.6/cli/php.ini \
&& sed -e "s/^max_execution_time .*$/max_execution_time = 60/g" -i /etc/php_origin/5.6/cli/php.ini \
&& export TMP=`echo "error_log\=${PHP5_LOG_FOLDER}/error.log" | sed -e "s/\//\\\\\\\\\//g"`  \
&& sed -e "s/^\;error_log = php_errors\.log$/$TMP/g" -i /etc/php_origin/5.6/cli/php.ini \
&& wget -O /usr/local/bin/phpunit https://phar.phpunit.de/phpunit.phar \
&& chmod +x /usr/local/bin/phpunit \
&& curl -sS https://getcomposer.org/installer | php -d allow_url_fopen=1 -- --filename=composer -- --install-dir=/usr/local/bin \
&& chmod +x /usr/local/bin/composer \
&& COMPOSER_HOME=/opt/drush COMPOSER_BIN_DIR=/usr/local/bin COMPOSER_VENDOR_DIR=/opt/drush/6 php -d disable_functions=" " -d allow_url_fopen=1 $(which composer) require drush/drush:6.* \
&& apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
COPY run.sh /run.sh
EXPOSE 80
EXPOSE 443

CMD ["/run.sh"]

