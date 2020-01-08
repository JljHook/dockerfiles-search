#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM php:5.6-fpm

MAINTAINER Clément Blanco <clement@cyber-duck.co.uk>

#
#--------------------------------------------------------------------------
# Software's Installation
#--------------------------------------------------------------------------
#
# Installing tools and PHP extentions using "apt", "docker-php", "pecl",
#

# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng12-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN apt-get update && \
    apt-get install -y --force-yes --no-install-recommends \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng12-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        openssh-server \
        swig \
        cmake \
        wget \
        git \
        cron \
        nano

# Install the PHP mcrypt extention
RUN docker-php-ext-install mcrypt

# Install the PHP pdo_mysql extention
RUN docker-php-ext-install pdo_mysql

# Install the PHP pdo_pgsql extention
RUN docker-php-ext-install pdo_pgsql

#####################################
# GD:
#####################################

# Install the PHP gd library
RUN docker-php-ext-install gd && \
    docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

#####################################
# PDFNetPHP by PDF Tron:
#####################################

# Create a temporary folder for this
RUN mkdir ~/PDFNetPHPSetup && \
    # Download the wrappers repo and extract it
    cd ~/PDFNetPHPSetup && wget https://github.com/PDFTron/PDFNetWrappers/archive/master.tar.gz && \
    tar xzvf master.tar.gz && \
    # Move to the correct folder before compiling
    cd PDFNetWrappers-master/PDFNetC && \
    # Download the PDFNet library and extract it here
    wget http://www.pdftron.com/downloads/PDFNetC64.tar.gz && tar xzvf PDFNetC64.tar.gz && \
    # Prepare for compilation
    mv PDFNetC64/Headers/ . && mv PDFNetC64/Lib/ . && \
    cd .. && mkdir Build && cd Build && \
    # Compiling...
    cmake -D BUILD_PDFNetPHP=ON .. && make && make install && \
    # Installing the PHP extension
    docker-php-ext-enable PDFNetPHP

#####################################
# Final Touch:
#####################################

RUN rm -r /var/lib/apt/lists/*
    
RUN usermod -u 1000 www-data

WORKDIR /var/www

EXPOSE 9000

CMD ["php-fpm"]
