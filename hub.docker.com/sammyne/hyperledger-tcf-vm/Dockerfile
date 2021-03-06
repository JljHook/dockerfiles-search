# Copyright 2019 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ------------------------------------------------------------------------------

# Description:
#   Builds the environment needed to build Trusted Compute Framework.
#
#  Configuration (build) parameters
#  - proxy configuration: https_proxy http_proxy ftp_proxy
#  - sgx mode:
#
# Build:
#   $ docker build docker -f docker/Dockerfile.tcf-dev -t tcf-dev
#   if behind a proxy, you might want to add also below options
#   --build-arg https_proxy=$https_proxy --build-arg http_proxy=$http_proxy --build-arg ftp_proxy=$ftp_proxy
#
# Run:
#   $ cd TrustedComputeFramework
#   $ docker run -v $(pwd):/project/TrustedComputeFramework -it tcf-dev /bin/bash
#   (to run with SGX HW, add options '--device=/dev/isgx -v /var/run/aesmd:/var/run/aesmd ')
#   make clean && make


FROM ubuntu:bionic

# Ignore timezone prompt in apt
ENV DEBIAN_FRONTEND=noninteractive

# Add necessary packages
RUN apt-get update \
   && apt-get install -y -q\
   build-essential \
   ca-certificates \
   cmake \
   git \
   make \
   pkg-config \
   cmake \
   protobuf-compiler \
   libprotobuf-dev \
   python \
   python3-dev \
   virtualenv \
   swig \
   software-properties-common \
   curl \
   dh-autoreconf \
   ocaml \
   wget \
   xxd \
   unzip \
   tar \
   liblmdb-dev \
   ocamlbuild \
   python3-tk \
   && apt-get -y -q upgrade \
   && apt-get clean \
   && rm -rf /var/lib/apt/lists/*

# SGX common library and SDK are installed in /opt/intel directory.
# Installation of SGX libsgx-common packages requires /etc/init directory. In docker image this directory doesn't exist.
# Hence creating /etc/init directory.
RUN mkdir -p /opt/intel \
   && mkdir -p /etc/init
WORKDIR /opt/intel

# Install SGX common library
RUN wget https://download.01.org/intel-sgx/linux-2.3.1/ubuntu18.04/libsgx-enclave-common_2.3.101.46683-1_amd64.deb \
   && dpkg -i libsgx-enclave-common_2.3.101.46683-1_amd64.deb

# Install SGX SDK
RUN wget https://download.01.org/intel-sgx/linux-2.3.1/ubuntu18.04/sgx_linux_x64_sdk_2.3.101.46683.bin \
   && chmod +x sgx_linux_x64_sdk_2.3.101.46683.bin \
   && echo "yes" | ./sgx_linux_x64_sdk_2.3.101.46683.bin \
   && rm sgx_linux_x64_sdk_2.3.101.46683.bin \
   && echo ". /opt/intel/sgxsdk/environment" >> /etc/environment

# ("Untrusted") OpenSSL
WORKDIR /tmp
RUN wget https://www.openssl.org/source/openssl-1.1.1b.tar.gz \
   && tar -zxvf openssl-1.1.1b.tar.gz \
   && cd openssl-1.1.1b/ \
   && ./config \
   && THREADS=8 \
   && make -j$THREADS \
   && make test \
   && make install -j$THREADS \
   && ldconfig \
   && cd .. \
   && rm -rf openssl-1.1.1b
# Note: we do _not_ delete openssl-1.1.1b.tar.gz as we re-use it below ..

RUN mkdir -p $HOME/.py-solc/solc-v0.4.25/bin \
   && curl -LsS https://github.com/ethereum/solidity/releases/download/v0.4.25/solc-static-linux -o /root/.py-solc/solc-v0.4.25/bin/solc \
   && chmod 0755 /root/.py-solc/solc-v0.4.25/bin/solc

# ("trusted") SGX OpenSSL
# Note: This will compile in HW or SIM mode depending on the availability of
# /dev/isgx and /var/run/aesmd/aesm.socket
WORKDIR /tmp
RUN git clone https://github.com/intel/intel-sgx-ssl.git  \
   && . /opt/intel/sgxsdk/environment \
   && (cd intel-sgx-ssl/openssl_source; mv /tmp/openssl-1.1.1b.tar.gz . ) \
   && (cd intel-sgx-ssl/Linux; if ([ -c /dev/isgx ] && [ -S /var/run/aesmd/aesm.socket ]); then SGX_MODE=HW; else SGX_MODE=SIM; fi; make SGX_MODE=${SGX_MODE} DESTDIR=/opt/intel/sgxssl all test ) \
   && (cd intel-sgx-ssl/Linux; make install ) \
   && rm -rf /tmp/intel-sgx-ssl \
   && echo "SGX_SSL=/opt/intel/sgxssl" >> /etc/environment

# Environment setup
WORKDIR /project/TrustedComputeFramework

RUN \
   echo "VIRTUAL_ENV=/project/TrustedComputeFramework/tools/build/_dev" >> /etc/environment \
   && echo "TCF_HOME=/project/TrustedComputeFramework/" >> /etc/environment \
   && if [ ! -z "$http_proxy"  ]; then \
   echo 'Acquire::http::Proxy "'$http_proxy'";' >> /etc/apt/apt.conf.d/00proxy; \
   echo "http_proxy=$http_proxy" >> /etc/wgetrc; \
   echo "http_proxy=$http_proxy" >> /etc/environment; \
   echo "HTTP_PROXY=$(echo $http_proxy | sed 's,[a-zA-Z]*://,,')" >> /etc/environment; \
   fi \
   && if [ ! -z "$ftp_proxy"  ];  then \
   echo 'Acquire::ftp::Proxy "'$ftp_proxy'";' >> /etc/apt/apt.conf.d/00proxy; \
   echo "ftp_proxy=$ftp_proxy" >> /etc/wgetrc; \
   echo "ftp_proxy=$ftp_proxy" >> /etc/environment; \
   fi \
   && if [ ! -z "$https_proxy" ]; then \
   echo 'Acquire::https::Proxy "'$https_proxy'";' >> /etc/apt/apt.conf.d/00proxy; \
   echo "https_proxy=$https_proxy" >> /etc/wgetrc; \
   echo "https_proxy=$https_proxy" >> /etc/environment; \
   echo "HTTPS_PROXY=$(echo $https_proxy | sed 's,[a-zA-Z]*://,,')" >> /etc/environment; \
   fi \
   && sed -i '1s;^;source /etc/environment\nexport $(grep -v "^. " /etc/environment| cut -d= -f1)\n;' /etc/environment

ENV http_proxy=$http_proxy
ENV https_proxy=$https_proxy
ENV TCF_HOME=/project/TrustedComputeFramework
ENV SGX_SSL=/opt/intel/sgxssl
ENV SGX_SDK=/opt/intel/sgxsdk
ENV PATH=$PATH:$SGX_SDK/bin:$SGX_SDK/bin/x64
ENV PKG_CONFIG_PATH=$PKG_CONFIG_PATH:$SGX_SDK/pkgconfig
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$SGX_SDK/sdk_libs
# Solc compiler path to compile solidity contract code
ENV SOLC_BINARY=/root/.py-solc/solc-v0.4.25/bin/solc
# Ethereum private key of ethereum account
ENV WALLET_PRIVATE_KEY="B413189C95B48737AE2D9AF4CAE97EB03F4DE40599DF8E6C89DCE4C2E2CBA8DE"
# Set display, replace value with [IP address of host]:0
ENV DISPLAY=:0

WORKDIR /project/TrustedComputeFramework