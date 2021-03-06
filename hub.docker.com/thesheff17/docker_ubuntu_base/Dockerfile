FROM ubuntu:16.04

MAINTAINER Dan Sheffner <Dan@Sheffner.com>

# install local-gen
RUN apt-get clean && apt-get update && apt-get install -y locales && \
    apt-get autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# helper ENV variables
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV EDITOR vim
ENV SHELL bash

# build date
RUN echo `date` > /root/build_date.txt

RUN \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -yq curl wget python-pip python-dev \
    git python3-pip python3-dev build-essential gcc vim \
    lsb-release mysql-client libpq-dev libjpeg-dev ffmpeg tmux \
    screen php-mcrypt php-mysql php php-cli pypy libffi-dev \
    libssl-dev openssl openjdk-8-jdk libmemcached-dev nodejs npm \
    zlib1g-dev libssl-dev libreadline-dev libyaml-dev \
    sudo liblttng-ust-ctl2 liblttng-ust0 libunwind8 liburcu4 \
    libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev \
    python-software-properties libgdbm-dev libncurses5-dev \
    automake libtool bison libffi-dev gawk libgmp-dev pkg-config \
    gcc make libffi-dev pkg-config libz-dev libbz2-dev tcl-dev \
    libsqlite3-dev libncurses-dev libexpat1-dev libssl-dev pypy mercurial \
    libgc-dev liblzma-dev tcl8.4-dev tk8.4-dev tk-dev net-tools man && \
    apt-get autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["/bin/bash"]
