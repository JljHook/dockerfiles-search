#
# Copyright (c) .NET Foundation and contributors. All rights reserved.
# Licensed under the MIT license. See LICENSE file in the project root for full license information.
#

# Dockerfile that creates a container suitable to build dotnet-cli
FROM centos:7.1.1503

RUN yum -q -y install deltarpm
RUN yum -q -y install epel-release
# RUN yum -y update

# This could become a "microsoft/coreclr" image, since it just installs the dependencies for CoreCLR (and stdlib)
# Install CoreCLR and CoreFx dependencies
RUN yum -q -y install unzip libunwind gettext libcurl-devel openssl-devel zlib libicu-devel

# RUN apt-get update && \
#     apt-get -qqy install unzip curl libicu-dev libunwind8 gettext libssl-dev libcurl3-gnutls zlib1g liblttng-ust-dev lldb-3.6-dev lldb-3.6 

# Install Build Prereqs
# CMake 3.3.2 from GhettoForge; LLVM 3.6.2 built from source ourselves;
RUN yum install -y http://mirror.symnds.com/distributions/gf/el/7/plus/x86_64/cmake-3.3.2-1.gf.el7.x86_64.rpm \
        https://matell.blob.core.windows.net/rpms/clang-3.6.2-1.el7.centos.x86_64.rpm \
        https://matell.blob.core.windows.net/rpms/clang-libs-3.6.2-1.el7.centos.x86_64.rpm \
        https://matell.blob.core.windows.net/rpms/lldb-3.6.2-1.el7.centos.x86_64.rpm \
        https://matell.blob.core.windows.net/rpms/lldb-devel-3.6.2-1.el7.centos.x86_64.rpm \
        https://matell.blob.core.windows.net/rpms/llvm-3.6.2-1.el7.centos.x86_64.rpm \
        https://matell.blob.core.windows.net/rpms/llvm-libs-3.6.2-1.el7.centos.x86_64.rpm \
        which \
        make

RUN yum -q -y install tar git

# Use clang as c++ compiler
RUN update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang++ 100
RUN update-alternatives --set c++ /usr/bin/clang++

RUN yum -q -y install sudo

# Clone Repo
RUN git clone https://github.com/dotnet/cli /opt/code/cli

# Set working directory
WORKDIR /opt/code/cli

# Default to bash
CMD bash


