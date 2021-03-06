FROM centos:6
MAINTAINER Virgil Security Inc. <support@virgilsecurity.com>

RUN yum install -y epel-release
RUN yum install -y centos-release-scl-rh
RUN yum install -y https://centos6.iuscommunity.org/ius-release.rpm
RUN yum groupinstall -y 'Development Tools'
RUN yum install -y pcre-devel
RUN yum remove -y git
RUN yum install -y git2u
RUN yum install -y curl wget
RUN yum install -y openssl-devel
RUN wget http://people.centos.org/tru/devtools-2/devtools-2.repo -O /etc/yum.repos.d/devtools-2.repo
RUN yum install -y devtoolset-2-gcc* devtoolset-2-binutils

# PHP
RUN yum install -y http://rpms.remirepo.net/enterprise/remi-release-6.rpm
RUN yum install -y --enablerepo=remi php56 php56-php-devel php56-php-mbstring
RUN yum install -y --enablerepo=remi rh-php70 rh-php70-php-devel rh-php70-php-mbstring
RUN yum install -y --enablerepo=remi php71 php71-php-devel php71-php-mbstring

# Python
RUN yum install -y python27 python27-dev
RUN yum install -y python34 python34-dev
RUN yum install -y rh-python35 rh-python35-dev
RUN yum install -y rh-python36 rh-python36-dev


ENV SWIG_VERSION=3.0.12
ENV CMAKE_VERSION_MAJOR=3
ENV CMAKE_VERSION_MINOR=11
ENV CMAKE_VERSION_PATCH=1
ENV CMAKE_VERSION=${CMAKE_VERSION_MAJOR}.${CMAKE_VERSION_MINOR}.${CMAKE_VERSION_PATCH}
ENV PHPUNIT_VERSION=5.7

RUN cd && \
    wget http://downloads.sourceforge.net/swig/swig-${SWIG_VERSION}.tar.gz && \
    tar xvfz swig-${SWIG_VERSION}.tar.gz && \
    cd swig-${SWIG_VERSION} && \
    ./configure --prefix=/usr --without-clisp --without-maximum-compile-warnings && \
    make -j4 && \
    make install && \
    install -v -m755 -d /usr/share/doc/swig-${SWIG_VERSION} && \
    cp -v -R Doc/* /usr/share/doc/swig-${SWIG_VERSION}

RUN cd && \
    wget https://cmake.org/files/v${CMAKE_VERSION_MAJOR}.${CMAKE_VERSION_MINOR}/cmake-${CMAKE_VERSION}-Linux-x86_64.sh && \
    bash cmake-${CMAKE_VERSION}-Linux-x86_64.sh --skip-license --exclude-subdir --prefix=/usr/local


RUN cd && \
    wget https://phar.phpunit.de/phpunit-${PHPUNIT_VERSION}.phar && \
    chmod +x phpunit-${PHPUNIT_VERSION}.phar && \
    mv phpunit-${PHPUNIT_VERSION}.phar /usr/local/bin/phpunit

ENTRYPOINT source /opt/rh/devtoolset-2/enable && /bin/bash
