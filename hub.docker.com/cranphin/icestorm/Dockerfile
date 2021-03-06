FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    bison \
    build-essential \
    clang \
    cmake \
    flex \
    gawk \
    git \
    graphviz \
    libboost-all-dev \
    libeigen3-dev \
    libffi-dev \
    libftdi-dev \
    libreadline-dev \
    mercurial \
    pkg-config \
    python \
    python3 \
    python3-dev \
    qt5-default \
    tcl-dev \
    xdot \
&& rm -rf /var/lib/apt/lists/*

RUN git clone --recursive https://github.com/cliffordwolf/icestorm.git
WORKDIR icestorm
RUN make -j$(nproc) && make install
WORKDIR /

RUN git clone --recursive https://github.com/cseed/arachne-pnr.git
WORKDIR arachne-pnr
RUN make -j$(nproc) && make install
WORKDIR /

RUN git clone --recursive https://github.com/YosysHQ/nextpnr
WORKDIR nextpnr
RUN cmake -DARCH=ice40 -DCMAKE_INSTALL_PREFIX=/usr/local . && make -j$(nproc) && make install
WORKDIR /

RUN git clone --recursive https://github.com/cliffordwolf/yosys.git
WORKDIR yosys
RUN make -j$(nproc) && make install
WORKDIR /

CMD [ "/bin/bash" ]
