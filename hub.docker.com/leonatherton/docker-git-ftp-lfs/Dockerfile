FROM ubuntu:18.04
MAINTAINER Leon Atherton <leon@leonatherton.com>


# install git and other deps
RUN apt-get -qq update && apt-get -qq install git wget libssl-dev build-essential software-properties-common

# add the newest git-ftp PPA and update the installation
RUN add-apt-repository ppa:git-ftp/ppa && apt-get update && apt-get -qq install git-ftp

# get and build libssh2 and curl
RUN wget https://www.libssh2.org/download/libssh2-1.9.0.tar.gz
RUN tar -xvzf libssh2-1.9.0.tar.gz
RUN cd libssh2-1.9.0 && ./configure && make && make install
RUN wget https://curl.haxx.se/download/curl-7.65.3.tar.bz2
RUN tar -xvjf curl-7.65.3.tar.bz2
RUN cd curl-7.65.3 && ./configure --with-libssh2=/usr/local && make && make install
RUN ldconfig

# get and install git lfs
RUN curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash
RUN apt-get -qq install git-lfs
RUN git lfs install
