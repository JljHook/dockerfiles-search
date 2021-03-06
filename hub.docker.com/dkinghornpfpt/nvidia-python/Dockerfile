FROM ubuntu:16.04

##########################
## All apt-get installs (tools, nvida requirements, cuda, etc.)
##########################
ENV CUDA_VERSION 8.0.61
ENV CUDA_PKG_VERSION 8-0=$CUDA_VERSION-1
ENV CUDNN_VERSION 5.1.10
RUN NVIDIA_GPGKEY_SUM=d1be581509378368edeec8c1eb2958702feedf3bc3d17011adbf24efacce4ab5 && \
    NVIDIA_GPGKEY_FPR=ae09fe4bbd223a84b2ccfce3f60f4b3d7fa2af80 && \
    apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub && \
    apt-key adv --export --no-emit-version -a $NVIDIA_GPGKEY_FPR | tail -n +5 > cudasign.pub && \
    echo "$NVIDIA_GPGKEY_SUM  cudasign.pub" | sha256sum -c --strict - && rm cudasign.pub && \
    echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/cuda.list && \
    echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list

RUN apt-get update && apt-get install -y --no-install-recommends\
    autoconf \
    automake \
    bc \
    build-essential \
    bzip2 \
    ca-certificates \
    cuda-cublas-8-0=8.0.61.1-1 \
    cuda-cudart-$CUDA_PKG_VERSION \
    cuda-cufft-$CUDA_PKG_VERSION \
    cuda-curand-$CUDA_PKG_VERSION \
    cuda-cusolver-$CUDA_PKG_VERSION \
    cuda-cusparse-$CUDA_PKG_VERSION \
    cuda-npp-$CUDA_PKG_VERSION \
    cuda-nvgraph-$CUDA_PKG_VERSION \
    cuda-nvrtc-$CUDA_PKG_VERSION \
    curl \
    dpkg-dev \
    file \
    g++ \
    gcc \
    gdb \
    git \
    imagemagick \
    less \
    libbz2-dev \
    libc6-dev \
    libcudnn5=$CUDNN_VERSION-1+cuda8.0 \
    libcurl4-openssl-dev \
    libdb-dev \
    libevent-dev \
    libffi-dev \
    libfreetype6-dev \
    libgdbm-dev \
    libgeoip-dev \
    libglib2.0-dev \
    libjpeg-dev \
    libkrb5-dev \
    liblzma-dev \
    libmagickcore-dev \
    libmagickwand-dev \
    libncurses-dev \
    libpng-dev \
    libpng12-dev \
    libpq-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    libtool \
    libunwind-dev \
    libwebp-dev \
    libxml2-dev \
    libxslt-dev \
    libyaml-dev \
    libzmq3-dev \
    make \
    module-init-tools \
    openssh-client \
    patch \
    pkg-config \
    procps \
    rsync \
    software-properties-common \
    tcl \
    tk \
    unzip \
    vim \
    wget \
    xz-utils \
    zlib1g-dev \
    $( \
        if apt-cache show 'default-libmysqlclient-dev' 2>/dev/null | grep -q '^Version:'; then \
            echo 'default-libmysqlclient-dev'; \
        else \
            echo 'libmysqlclient-dev'; \
        fi \
    ) \
	; \
    apt-get autoremove \
    && apt-get clean \
    && rm -rf /var/tmp /var/lib/apt/lists/* \
    && rm -rf /tmp/*

##########################
## NVIDIA Driver install
##########################
ENV DRIVER_VERSION 375.51

RUN  mkdir -p /usr/src/kernels \
    && cd /usr/src/kernels \
    && git clone -q git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git --single-branch --depth 1 --branch v`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]*$//"`  linux \
    && cd linux \
    && git checkout -qb stable v`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]*$//"` \
    && zcat /proc/config.gz > .config \
    && make modules_prepare \
    && sed -i -e "s/`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]??*$//"`/`uname -r`/" include/generated/utsrelease.h # In case a '+' was added

RUN mkdir -p /opt/nvidia && cd /opt/nvidia/ \
    && wget -q http://us.download.nvidia.com/XFree86/Linux-x86_64/${DRIVER_VERSION}/NVIDIA-Linux-x86_64-${DRIVER_VERSION}.run -O /opt/nvidia/driver.run \
    && chmod +x /opt/nvidia/driver.run \
    && /opt/nvidia/driver.run -a -x --ui=none

ENV NVIDIA_INSTALLER /opt/nvidia/NVIDIA-Linux-x86_64-${DRIVER_VERSION}/nvidia-installer
CMD ${NVIDIA_INSTALLER} -q -a -n -s --kernel-source-path=/usr/src/kernels/linux/ \
    && modprobe nvidia \
    && modprobe nvidia-uvm

##########################
## NVIDIA CUDA install
##########################
LABEL com.nvidia.cuda.version="${CUDA_VERSION}"

RUN /opt/nvidia/driver.run --silent --no-kernel-module --no-unified-memory --no-opengl-files

RUN ln -s cuda-8.0 /usr/local/cuda

RUN echo "/usr/local/cuda/lib64" >> /etc/ld.so.conf.d/cuda.conf && \
    ldconfig

RUN echo "/usr/local/nvidia/lib" >> /etc/ld.so.conf.d/nvidia.conf && \
    echo "/usr/local/nvidia/lib64" >> /etc/ld.so.conf.d/nvidia.conf

ENV PATH /usr/local/nvidia/bin:/usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64

##########################
## PYTHON 3 install
##########################

ENV GPG_KEY 0D96DF4D4110E5C43FBFB17F2D347EA6AA65421D
ENV PYTHON_VERSION 3.6.1

RUN set -ex \
	&& buildDeps=' \
		dpkg-dev \
		tcl-dev \
		tk-dev \
	' \
	&& apt-get update && apt-get install -y $buildDeps --no-install-recommends && rm -rf /var/lib/apt/lists/* \
	\
	&& wget -qO python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
	&& wget -qO python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" \
	&& gpg --batch --verify python.tar.xz.asc python.tar.xz \
	&& rm -r "$GNUPGHOME" python.tar.xz.asc \
	&& mkdir -p /usr/src/python \
	&& tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
	&& rm python.tar.xz \
	\
	&& cd /usr/src/python \
	&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
	&& ./configure \
		--build="$gnuArch" \
		--enable-loadable-sqlite-extensions \
		--enable-shared \
		--without-ensurepip \
	&& make -j "$(nproc)" \
	&& make install \
	&& ldconfig \
	\
	&& apt-get purge -y --auto-remove $buildDeps \
	\
	&& find /usr/local -depth \
		\( \
			\( -type d -a -name test -o -name tests \) \
			-o \
			\( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
		\) -exec rm -rf '{}' + \
	&& rm -rf /usr/src/python \
    && apt-get clean \
    && rm -rf /var/tmp /var/lib/apt/lists/* \
    && rm -rf /tmp/*

## Create symlinks
RUN cd /usr/local/bin \
	&& ln -s idle3 idle \
	&& ln -s pydoc3 pydoc \
	&& ln -s python3 python \
	&& ln -s python3-config python-config

##########################
## Install Pip
##########################
ENV PYTHON_PIP_VERSION 9.0.1

RUN set -ex; \
	wget -qO get-pip.py 'https://bootstrap.pypa.io/get-pip.py'; \
	python get-pip.py \
		--disable-pip-version-check \
		--no-cache-dir \
		"pip==$PYTHON_PIP_VERSION" \
	; \
	pip --version; \
	find /usr/local -depth \
		\( \
			\( -type d -a -name test -o -name tests \) \
			-o \
			\( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
		\) -exec rm -rf '{}' +; \
	rm -f get-pip.py


##########################
## Misc
##########################
## For CUDA profiling, TensorFlow requires CUPTI.
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

