FROM ubuntu:xenial

ENV DEBIAN_FRONTEND noninteractive

COPY resolv.conf /etc

RUN apt-get update && apt-get upgrade -y
RUN apt-get -y install apache2 apache2-dev mysql-server libmysqlclient-dev
RUN apt-get install -y php7.0-mysql php7.0-curl php7.0-json php7.0-cgi php7.0 libapache2-mod-php7.0 php-pear autoconf automake curl build-essential libxslt1-dev re2c \
	libxml2 libxml2-dev php7.0-cli bison libbz2-dev libreadline-dev libcurl4-gnutls-dev
RUN apt-get install -y libfreetype6 libfreetype6-dev libpng12-0 libpng12-dev libjpeg-dev libjpeg8-dev \
	libjpeg8  libgd-dev libgd3 libxpm4 libltdl7 libltdl-dev
RUN apt-get install -y libssl-dev openssl gettext libgettextpo-dev libgettextpo0 libicu-dev libmhash-dev \
	libmhash2 libmcrypt-dev libmcrypt4 wget vim git locales

# For Number formatter tests
RUN locale-gen en
RUN locale-gen de
RUN locale-gen de_DE
RUN locale-gen uk
RUN locale-gen ru

# For RVM
RUN apt-get -y install libreadline6-dev zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 \
	libxml2-dev libxslt1-dev autoconf libgdbm-dev libncurses5-dev automake libtool bison pkg-config libffi-dev

# QT 4
RUN apt-get -y install libqt4-dev libqtwebkit-dev
RUN apt-get -y install xvfb

RUN curl -sS https://getcomposer.org/installer | php
RUN chmod +x composer.phar
RUN mv composer.phar /usr/local/bin/composer

RUN apt-get clean -y; \
    apt-get autoclean -y; \
    apt-get autoremove -y; \
    rm -rf /var/lib/{apt,dpkg,cache,log}/

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

COPY apache_default /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite

RUN echo insecure >> ~/.curlrc
RUN curl -L -O https://github.com/phpbrew/phpbrew/raw/master/phpbrew
RUN chmod +x phpbrew
RUN mv phpbrew /usr/bin/phpbrew
RUN chmod -R oga+rw /usr/lib/apache2/modules
RUN chmod -R oga+rw /var/lib/apache2/module
RUN chmod -R oga+rw /etc/apache2

#RUN mkdir /home/ellislab
#RUN groupadd -r ellislab -g 433 && \
#useradd -u 431 -r -g ellislab -d /home/ellislab -s /bin/bash -c "Docker image user" ellislab && \
#chown -R ellislab:ellislab /home/ellislab && \
#chown -R ellislab:ellislab /var/www/html
#USER ellislab
#WORKDIR /home/ellislab

RUN phpbrew init
RUN bash -l -c 'source ~/.phpbrew/bashrc'

ENV phpbrewopts +default+mysql+apxs2=/usr/bin/apxs2 -- --with-libdir=lib/x86_64-linux-gnu --with-gd=shared --enable-gd-natf --with-jpeg-dir=/usr --with-png-dir=/usr
RUN phpbrew install 5.6.0 $phpbrewopts
RUN phpbrew install 7.2.11 $phpbrewopts

RUN ls /root/.phpbrew/php/php-*/etc/php.ini | xargs sed -i "s/\;date\.timezone\ \=/date\.timezone\ \=\ America\/New_York/g"
RUN ls /root/.phpbrew/php/php-*/etc/php.ini | xargs sed -i "s/\;phar.readonly.*/phar.readonly = Off/g"

# Install extensions
RUN bash -l -c 'source ~/.phpbrew/bashrc && phpbrew switch php-5.6.0 && phpbrew ext install gd && phpbrew ext install intl'
RUN bash -l -c 'source ~/.phpbrew/bashrc && phpbrew switch php-7.2.11 && phpbrew ext install gd && phpbrew ext install intl'

RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
RUN bash -l -c 'curl -sSL https://get.rvm.io | bash -s stable'
RUN bash -l -c 'rvm autolibs disable'
RUN bash -l -c 'rvm requirements'
RUN bash -l -c 'rvm install 2.2.3'
RUN bash -l -c 'rvm use 2.2.3'
RUN bash -l -c 'gem install bundler'

COPY run.sh /
RUN chmod +x run.sh
COPY updategems.sh /
RUN chmod +x updategems.sh
