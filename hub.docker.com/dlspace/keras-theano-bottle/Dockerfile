FROM alpine:latest

MAINTAINER sreedhar potturi <sreedhar.potturi@gmail.com>

RUN apk --update  --repository http://dl-4.alpinelinux.org/alpine/edge/community add \
    bash \
    git \
    curl \
    ca-certificates \
    bzip2 \
    unzip \
    sudo \
    libstdc++ \
    glib \
    libxext \
    libxrender \
    tini \ 
    python-dev \ 
    g++ \ 
    && curl -L "https://github.com/andyshinn/alpine-pkg-glibc/releases/download/2.25-r0/glibc-2.25-r0.apk" -o /tmp/glibc.apk \
    && curl -L "https://github.com/andyshinn/alpine-pkg-glibc/releases/download/2.25-r0/glibc-bin-2.25-r0.apk" -o /tmp/glibc-bin.apk \
    && curl -L "https://github.com/andyshinn/alpine-pkg-glibc/releases/download/2.25-r0/glibc-i18n-2.25-r0.apk" -o /tmp/glibc-i18n.apk \
    && apk add --allow-untrusted /tmp/glibc*.apk \
    && /usr/glibc-compat/sbin/ldconfig /lib /usr/glibc-compat/lib \
    && /usr/glibc-compat/bin/localedef -i en_US -f UTF-8 en_US.UTF-8 \
    && rm -rf /tmp/glibc*apk /var/cache/apk/*

# Configure environment
ENV CONDA_DIR=/opt/conda CONDA_VER=4.3.14
ENV PATH=$CONDA_DIR/bin:$PATH SHELL=/bin/bash LANG=C.UTF-8
ENV KERAS_BACKEND=theano

# Install conda
RUN mkdir -p $CONDA_DIR && \
    echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh && \
    curl https://repo.continuum.io/miniconda/Miniconda3-${CONDA_VER}-Linux-x86_64.sh  -o mconda.sh && \
    /bin/bash mconda.sh -f -b -p $CONDA_DIR && \
    rm mconda.sh && \
    $CONDA_DIR/bin/conda install --yes conda==${CONDA_VER} && \
    $CONDA_DIR/bin/conda install --yes numpy scipy mkl nose sphinx && \
    $CONDA_DIR/bin/pip install parameterized && \
    $CONDA_DIR/bin/conda install --yes theano pygpu keras && \
    $CONDA_DIR/bin/pip install bottle


