FROM centos:7
MAINTAINER Ron Williams <hello@ronwilliams.io>
ENV PATH /usr/local/src/vendor/bin/:/usr/local/rvm/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Set TERM env to avoid mysql client error message "TERM environment variable not set" when running from inside the container
ENV TERM xterm

# Fix command line compile issue with bundler.
ENV LC_ALL en_US.utf8

# Custom docroot (see conf/run-httpd.sh)
ENV DOCROOT /var/www/public

# Install and enable repositories
RUN yum -y update && \
    yum -y install epel-release && \
    rpm -Uvh https://centos7.iuscommunity.org/ius-release.rpm && \
    yum -y update

RUN yum -y install \
    curl \
    git2u \
    mariadb \
    msmtp \
    net-tools \
    python34 \
    vim \
    wget \
    rsync \
    unzip \
    gcc \
    gcc-c++ \
    make \
    php72u-devel

# Install PHP and PHP modules
RUN yum -y install \
    php72u \
    php72u-cli \
    php72u-curl \
    php72u-gd \
    php72u-imap \
    php72u-mbstring \
    php72u-mcrypt \
    php72u-mysql \
    php72u-odbc \
    php72u-pear \
    php72u-mysqlnd \
    php72u-pecl-imagick \
    php72u-pecl-json \
    php72u-pecl-zendopcache \
    php72u-redis \
    php72u-bcmath \
    php72u-xml \
    php72u-ldap

# Install misc tools
RUN yum -y install \
    python-setuptools

# Perform yum cleanup
RUN yum -y upgrade && \
    yum clean all

# Install Composer and Drush
RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/local/bin \
    --filename=composer \
    --version=1.2.0 && \
    composer \
    --working-dir=/usr/local/src/ \
    global \
    require \
    drush/drush:8.* && \
    ln -s /usr/local/src/vendor/bin/drush /usr/bin/drush

# We need to remove this because D8 sites can't use this extension
# RUN drush dl registry_rebuild-7.x

# Install Node.js and NPM
RUN yum -y install nodejs

# Install GRUNT tool and CLI tool
RUN npm install grunt@1.0.3 --save-dev
RUN npm install -g grunt-cli

# Disable services management by systemd.
RUN systemctl disable httpd.service

# Apache config, and PHP config, test apache config
# See https://github.com/docker/docker/issues/7511 /tmp usage
COPY public/index.php /var/www/public/index.php
COPY centos-7 /tmp/centos-7/

RUN rsync -a /tmp/centos-7/etc/ /etc/ && \
    apachectl configtest

EXPOSE 80 443

# Simple startup script to avoid some issues observed with container restart
ADD conf/run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh

ADD conf/mail.ini /etc/php.d/mail.ini
RUN chmod 644 /etc/php.d/mail.ini

CMD ["/run-httpd.sh"]
