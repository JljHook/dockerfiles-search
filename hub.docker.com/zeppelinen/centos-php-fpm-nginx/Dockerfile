FROM centos:7

SHELL ["/bin/bash", "-c"]

ARG git_user
ARG git_password

RUN yum -y --setopt=tsflags=nodocs update && \
    yum -y --setopt=tsflags=nodocs --nogpgcheck install epel-release && \
    yum -y --setopt=tsflags=nodocs --nogpgcheck install https://centos7.iuscommunity.org/ius-release.rpm && \
    yum -y --setopt=tsflags=nodocs --nogpgcheck install php72u-cli \
        php72u-fpm \
        php72u-bcmath \
        php72u-gd \
        php72u-intl \
        php72u-json \
        php72u-ldap  \
        php72u-mbstring \
        php72u-mcrypt \
        php72u-opcache \
        php72u-pdo \
        php72u-pear  \
        php72u-pecl-apcu \
        php72u-pecl-imagick \
        php72u-pecl-redis \
        php72u-pecl-xdebug  \
        php72u-pgsql \
        php72u-mysqlnd \
        php72u-soap \
        php72u-tidy \
        php72u-xml \
        php72u-xmlrpc \
	php72u-process \
        python-setuptools \ 
        git \
        npm \
        libpng12-1.2.50-10.el7.x86_64 \
        libpng12-devel-1.2.50-10.el7.x86_64 \
        nginx \
        python2-pip && \
        pip install supervisor && \
        curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer && \
        curl -sL https://rpm.nodesource.com/setup_8.x | bash - && \
        yum -y install libpng12-1.2.50-10.el7.x86_64 \
        libpng12-devel-1.2.50-10.el7.x86_64

COPY conf/php.ini /etc/php.ini
COPY conf/php-fpm.d/www.conf /etc/php-fpm.d/www.conf
COPY conf/nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
