FROM debian:jessie
MAINTAINER TM@TONIMICHEL.DE
RUN apt-get update && apt-get install -y locales \
    tar \
    git \
    curl \
    nano \
    wget \
    sudo \
    net-tools \
    build-essential \
    python3-yaml \
    python3-jinja2 \
    python3-httplib2 \
    python3-paramiko \
    python3-setuptools \
    python3-pkg-resources \
    python3-pip \
    python3-psycopg2 \
    bpython3 \
    postgresql-server-dev-9.4 \
    libncurses-dev \
    libpython3.4-stdlib \
    libpython3.4-dev \
    python3-venv \
    python3.4-dev \
    python3 \
    python-virtualenv \
    cmake \
    libtiff5-dev \
    libjpeg62-turbo-dev \
    zlib1g-dev \
    libfreetype6-dev \
    liblcms2-dev \
    libwebp-dev \
    tcl8.6-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
	&& rm -rf /var/lib/apt/lists/*

ENV CONTAINER_USERNAME='app'
ENV CONTAINER_GROUPNAME='app'
ENV HOMEDIR='/home/'$CONTAINER_USERNAME

# add non root user
RUN  adduser --disabled-password --gecos '' $CONTAINER_USERNAME && \
  adduser $CONTAINER_USERNAME sudo && \
  chown -R $CONTAINER_USERNAME:$CONTAINER_GROUPNAME $HOMEDIR

# dirs and permissions
RUN mkdir -p /usr/src/$CONTAINER_USERNAME &&\
  mkdir -p /srv/$CONTAINER_USERNAME &&\
  chown $CONTAINER_USERNAME /usr/src/$CONTAINER_USERNAME -R && \
  chown $CONTAINER_USERNAME $HOMEDIR && \
  chown $CONTAINER_USERNAME /srv/$CONTAINER_USERNAME && \
  /usr/bin/pip3 install virtualenv

#change user
USER app
WORKDIR /usr/src/app
ENV LANG c.utf8
ENV LC_ALL c.utf8
ENV LANGUAGE en_US.utf8
RUN virtualenv -p python3 rAPPenv --no-site-packages
ENV VIRTUAL_ENV /usr/src/app/rAPPenv
ENV  PATH "$VIRTUAL_ENV/bin:$PATH"
RUN pip install bpython --upgrade
WORKDIR /srv/app/

CMD ["bpython"]
