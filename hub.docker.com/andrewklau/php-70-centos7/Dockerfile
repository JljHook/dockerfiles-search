FROM openshift/base-centos7

# This image provides an Apache+PHP environment for running PHP
# applications.

MAINTAINER Mateus Caruccio <mateus.caruccio@getupcloud.com>

EXPOSE 8080

ENV PHP_VERSION=7.0

LABEL io.k8s.description="Platform for building and running PHP 7.0 applications" \
      io.k8s.display-name="Apache 2.4 with PHP 7.0" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,php,php70"

# Install Apache httpd and PHP
RUN yum install -y epel-release http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    INSTALL_PKGS="php php-ast php-bcmath php-cli php-common php-dba php-gd php-intl php-json php-mbstring \
                  php-mcrypt php-mysqlnd php-opcache php-pdo php-pear php-pecl-igbinary \
                  php-pecl-memcache php-pecl-mongodb php-pecl-redis php-pecl-scrypt php-pecl-uuid php-pgsql \
                  urw-fonts libXext libXrender fontconfig" && \
    yum install -y --enablerepo=remi-php70,epel-release --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Each language image can have 'contrib' a directory with extra files needed to
# run and build the applications.
COPY ./contrib/ /opt/app-root

# In order to drop the root user, we have to make some directories world
# writeable as OpenShift default security model is to run the container under
# random UID.
RUN sed -i -f /opt/app-root/etc/httpdconf.sed /etc/httpd/conf/httpd.conf && \
    echo "IncludeOptional /opt/app-root/etc/conf.d/*.conf" >> /etc/httpd/conf/httpd.conf && \
    sed -i '/php_value session.save_path/d' /etc/httpd/conf.d/php.conf && \
    head -n151 /etc/httpd/conf/httpd.conf | tail -n1 | grep "AllowOverride All" || exit && \
    mkdir /tmp/sessions && \
    touch /etc/php.ini && \
    chown -R 1001:0 /opt/app-root /tmp/sessions /etc/php.d && \
    chmod -R a+rwx /tmp/sessions && \
    chmod -R ug+rwx /opt/app-root /etc/php.d && \
    chmod -R ug+rw /etc/php.ini && \
    chmod -R a+rwx /var/run/httpd

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
