FROM ubuntu:16.04

# Set proper locale
RUN apt-get clean && apt-get update
RUN apt-get install -y locales
RUN locale-gen en_US.UTF-8
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

# Installing xvfb and related stuff
# RUN apt-get install -y libxtst6
# RUN apt-get install -y libxext6
RUN apt-get update
RUN apt-get -f install
RUN apt-get install -fy xvfb

# Install Xvfb init script
ADD xvfb_init /etc/init.d/xvfb
RUN chmod a+x /etc/init.d/xvfb
RUN update-rc.d xvfb defaults
ADD xvfb-daemon-run /usr/bin/xvfb-daemon-run
RUN chmod a+x /usr/bin/xvfb-daemon-run

#Install all the languages/compilers we are supporting.
RUN apt-get install -y gcc g++ libjson-perl perl python

#Install ML libraries for python
RUN apt-get install -y python-dev python-numpy python3-numpy python-numpy-dev python3-numpy-dev python-setuptools python-scipy python3-scipy libatlas-dev python-pip python-pandas python3-pandas python3-pip python3-dev

#Install Mono C# and F#
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
RUN echo "deb http://download.mono-project.com/repo/ubuntu xenial main" | tee /etc/apt/sources.list.d/mono-official.list
RUN apt-get update
RUN apt-get install -y mono-mcs mono-devel mono-xsp4 mono-xsp4-base mono-complete ca-certificates-mono fsharp

#Install Haskell with TEXT.JSON package
RUN apt-get install -y haskell-platform
RUN cabal update && cabal install JSON

RUN apt-get install -y mono-vbnc
RUN apt-get install -y npm

RUN apt-get install -y sudo

RUN apt-get install -y python-software-properties
RUN apt-get install -y curl libcurl4-openssl-dev wget
#Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
RUN apt-get install -y nodejs

RUN apt-get install -y maven

RUN npm install -g sequelize underscore crypto-js lodash request shelljs http sys jquery async connect mysql mysql2 validator  ws co when forever  debug && export NODE_PATH=/usr/lib/node_modules/

#prepare for Java download
RUN apt-get install -y python-software-properties
RUN apt-get install -y software-properties-common

#Install GO
RUN add-apt-repository -y ppa:longsleep/golang-backports && apt-get update
RUN apt-get install -y golang-go

#grab oracle java (auto accept licence)
RUN add-apt-repository -y ppa:linuxuprising/java \
    && apt-get update \
    && echo oracle-java11-installer shared/accepted-oracle-license-v1-2 select true | /usr/bin/debconf-set-selections \
    && apt-get install -y --no-install-recommends \
        oracle-java11-installer \
        oracle-java11-set-default \
        libjackson2-core-java \
        libjackson2-databind-java \
        libmysql-java \
        maven \
    && rm -rf /var/lib/apt/lists/*

ENV CLASSPATH=/usr/share/java/*:$CLASSPATH

RUN wget http://www.scala-lang.org/files/archive/scala-2.12.5.deb
RUN dpkg -i scala-2.12.5.deb && apt-get update
RUN apt-get install -y scala

#Installing mysql server
RUN echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "mysql-server mysql-server/root_password password root" | debconf-set-selections
RUN echo "mysql-server mysql-server/root_password_again password root" | debconf-set-selections
RUN apt-get update && \
  apt-get -y install mysql-server-5.7 && \
  mkdir -p /var/lib/mysql && \
  mkdir -p /var/run/mysqld && \
  mkdir -p /var/log/mysql && \
  chown -R mysql:mysql /var/lib/mysql && \
  chown -R mysql:mysql /var/run/mysqld && \
  chown -R mysql:mysql /var/log/mysql
# UTF-8 and bind-address
RUN sed -i -e "$ a [client]\n\n[mysql]\n\n[mysqld]"  /etc/mysql/my.cnf && \
  sed -i -e "s/\(\[client\]\)/\1\ndefault-character-set = utf8/g" /etc/mysql/my.cnf && \
  sed -i -e "s/\(\[mysql\]\)/\1\ndefault-character-set = utf8/g" /etc/mysql/my.cnf && \
  sed -i -e "s/\(\[mysqld\]\)/\1\ninit_connect='SET NAMES utf8'\ncharacter-set-server = utf8\ncollation-server=utf8_unicode_ci\nbind-address = 0.0.0.0/g" /etc/mysql/my.cnf


RUN apt-get install -y bc
RUN echo "mysql ALL = NOPASSWD: /usr/sbin/service mysql start" | cat >> /etc/sudoers

#Installing Galen framework and related stuff
# RUN npm install -g galenframework-cli

#RUN add-apt-repository ppa:mozillateam/firefox-stable
#RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
#RUN apt-get update

#Installing Galen framework and related stuff
RUN apt-get install -y jq dbus-x11 unzip
RUN wget https://codefightsuserpics.s3.amazonaws.com/galen-ide.zip -P /tmp/
RUN unzip /tmp/galen-ide.zip -d /tmp && cd /tmp/galen-ide && . /tmp/galen-ide/assembleAndInstall.sh

#Install Ruby 2.5
RUN apt-add-repository ppa:brightbox/ruby-ng && apt-get update
RUN apt-get install -y ruby2.5

#Install PHP 7
RUN add-apt-repository ppa:ondrej/php && apt-get update
RUN apt-get install -y php7.2

#Install Firefox 46 version. This exact version is needed for galen-ide
RUN cd /usr/local && wget http://ftp.mozilla.org/pub/mozilla.org/firefox/releases/46.0.1/linux-x86_64/en-US/firefox-46.0.1.tar.bz2 && tar xvjf firefox-46.0.1.tar.bz2 && ln -s /usr/local/firefox/firefox /usr/bin/firefox
ENV DISPLAY :99

RUN apt-get install -y clang

#Install (latest) R
#For the latest versions of R
#RUN echo "deb http://cloud.r-project.org/bin/linux/ubuntu trusty/" > /etc/apt/sources.list
RUN add-apt-repository -y ppa:marutter/rrutter
#jsonlite is only available in default cran ubuntu repo starting from 16.04
RUN add-apt-repository -y ppa:marutter/c2d4u
RUN apt-get update
RUN apt-get install -y r-base
#RUN apt-get install -y r-cran-rjson # inconsistently parses single value lists
RUN apt-get install -y r-cran-stringr r-cran-jsonlite r-cran-httr

#Install D
RUN wget http://downloads.dlang.org/releases/2.x/2.079.1/dmd_2.079.1-0_amd64.deb
RUN dpkg -i dmd_2.079.1-0_amd64.deb
# Old unreliable way:
#RUN wget http://master.dl.sourceforge.net/project/d-apt/files/d-apt.list -O /etc/apt/sources.list.d/d-apt.list
#RUN wget -qO - https://dlang.org/d-keyring.gpg | apt-key add -
#RUN apt-get update
#RUN apt-get install -y --force-yes dmd-bin

#Install Common Lisp
RUN apt-get install -y libsigsegv2 libffcall1 libreadline5
#RUN apt-get install -y sbcl
RUN wget http://ftp.debian.org/debian/pool/main///c/clisp/clisp_2.49-8.1_amd64.deb
RUN dpkg -i clisp_2.49-8.1_amd64.deb
RUN wget https://beta.quicklisp.org/quicklisp.lisp -P /tmp
RUN echo "(quicklisp-quickstart:install)\n(ql:add-to-init-file)\n" >> /tmp/quicklisp.lisp
RUN printf '\n' | clisp /tmp/quicklisp.lisp
#RUN touch /tmp/install.lisp
#RUN echo "(load \"~/quicklisp/setup.lisp\")\n(ql:quickload \"alexandria\")\n(ql:quickload \"cl-ppcre\")\n" > /tmp/install.lisp
#RUN sbcl --non-interactive --load /tmp/install.lisp
#RUN clisp /tmp/install.lisp

#Install Lua
RUN apt-get install -y make libreadline-dev
RUN curl -R -O http://www.lua.org/ftp/lua-5.3.4.tar.gz
RUN tar zxf lua-5.3.4.tar.gz && cd lua-5.3.4 && make linux install
RUN wget http://luajit.org/download/LuaJIT-2.0.5.tar.gz
RUN tar -xvzf LuaJIT-2.0.5.tar.gz
RUN cd LuaJIT-2.0.5 && make && make install

#Install CoffeeScript
RUN npm install -g coffeescript@next

#Install Clojure
RUN add-apt-repository -y ppa:jonathonf/lisp && apt-get update
RUN apt-get install -y clojure

#Install Ocaml
RUN apt-get install -y ocaml ocaml-batteries-included

#Install Erlang
RUN wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
RUN dpkg -i erlang-solutions_1.0_all.deb
RUN apt-get update
RUN apt-get install -y --force-yes erlang
#Install High-Performance Erlang Project compiler
RUN apt-get install -y erlang-base-hipe

#Install latest Elixir
RUN apt-get install -y elixir

#Install Rust (latest stable version)
RUN mkdir -p /opt/rust && \
    curl https://sh.rustup.rs -sSf | HOME=/opt/rust sh -s -- --no-modify-path -y && \
    chmod -R 777 /opt/rust

#Install Kotlin
RUN apt-get install -y zip
ENV KOTLIN_VERSION 1.2.40
ENV KOTLIN_COMPILER_URL https://github.com/JetBrains/kotlin/releases/download/v${KOTLIN_VERSION}/kotlin-compiler-${KOTLIN_VERSION}.zip
RUN wget $KOTLIN_COMPILER_URL -O /tmp/a.zip && \
    unzip /tmp/a.zip -d /opt && \
    rm /tmp/a.zip
ENV PATH $PATH:/opt/kotlinc/bin

#Install Nim
RUN add-apt-repository -y ppa:jonathonf/nimlang && apt-get update
RUN apt-get install -y nim

#Install Julia 0.6.2, from https://github.com/docker-library/julia/blob/master/Dockerfile
ENV JULIA_PATH /usr/local/julia
ENV JULIA_VERSION 0.6.2

RUN mkdir $JULIA_PATH \
  && apt-get update && apt-get install -y curl \
  && curl -sSL "https://julialang-s3.julialang.org/bin/linux/x64/${JULIA_VERSION%[.-]*}/julia-${JULIA_VERSION}-linux-x86_64.tar.gz" -o julia.tar.gz \
  && curl -sSL "https://julialang-s3.julialang.org/bin/linux/x64/${JULIA_VERSION%[.-]*}/julia-${JULIA_VERSION}-linux-x86_64.tar.gz.asc" -o julia.tar.gz.asc \
  && export GNUPGHOME="$(mktemp -d)" \
  && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys 3673DF529D9049477F76B37566E3C7DC03D6E495 \
  && gpg --batch --verify julia.tar.gz.asc julia.tar.gz \
  && rm -r "$GNUPGHOME" julia.tar.gz.asc \
  && tar -xzf julia.tar.gz -C $JULIA_PATH --strip-components 1 \
  && rm -rf /var/lib/apt/lists/* julia.tar.gz*


ENV PATH $JULIA_PATH/bin:$PATH

#Install FPC
RUN apt-get update && apt-get install -y fpc

#Install Groovy

ENV GROOVY_VERSION 2.4.15
RUN cd /tmp && \
    wget http://dl.bintray.com/groovy/maven/apache-groovy-binary-${GROOVY_VERSION}.zip && \
    unzip apache-groovy-binary-${GROOVY_VERSION}.zip && \
    mv groovy-${GROOVY_VERSION} /opt/groovy && \
    rm apache-groovy-binary-${GROOVY_VERSION}.zip
ENV GROOVY_HOME /opt/groovy
ENV PATH $GROOVY_HOME/bin/:$PATH

#Install TypeScript
# For @types/node@9.6.7 see https://github.com/DefinitelyTyped/DefinitelyTyped/issues/25342
RUN npm install -g typescript @types/node@9.6.7

#Install Tcl
RUN apt-get install -y tcl8.6 tcllib

#Install Dart 1.24.2
# Enable HTTPS for apt.
RUN apt-get install -y apt-transport-https
# Get the Google Linux package signing key.
RUN curl https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
# Set up the location of the stable repository.
RUN curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > /etc/apt/sources.list.d/dart_stable.list
RUN apt-get update && apt-get install -y dart=1.24.2-1

#Install Smalltalk
RUN apt-get install -y gnu-smalltalk

#Install GNU Octave
RUN apt-get install -y octave

#Additional libs for python
RUN apt-get install -y libmysqlclient-dev
RUN pip install requests configparser mysql-connector MySql-Python SQLAlchemy
RUN pip3 install requests mysql-connector PyMySQL SQLAlchemy

#Additional libs for php
RUN apt-get update
RUN apt-get install -y php-curl
RUN apt-get install -y php-mysql

#Additional libs for CS
RUN wget https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-8.0.11-noinstall.zip -P /tmp/
RUN unzip /tmp/mysql-connector-net-8.0.11-noinstall.zip -d /tmp && cd /tmp/v4.5.2 && cp MySql.Data.dll /usr/lib/mono/ && cd /usr/lib/mono/ && gacutil -i MySql.Data.dll -package MySql.Data
RUN wget https://github.com/JamesNK/Newtonsoft.Json/releases/download/11.0.2/Json110r2.zip -P /tmp/
RUN rm -rf /tmp/Newtonsoft && unzip /tmp/Json110r2.zip -d /tmp/Newtonsoft || : && cd /tmp/Newtonsoft/Bin/net45 && cp Newtonsoft.Json.dll /usr/lib/mono/ && cd /usr/lib/mono/ && gacutil -i Newtonsoft.Json.dll -package Newtonsoft.Json

#MySQL and PostgreSQL libs for Java, C++ and Perl
RUN apt-get update
RUN apt-get install -y libmysql-java libssl-dev libdbi-perl libdbd-mysql-perl postgresql postgresql-contrib postgresql-client libpq5 libpq-dev

#Installing additional ruby libs
RUN apt-get install -y ruby`ruby -e 'puts RUBY_VERSION[/\d+\.\d+/]'`-dev libpq-dev
RUN gem install rspec pg mysql2 activemodel activerecord activesupport json faker pry
