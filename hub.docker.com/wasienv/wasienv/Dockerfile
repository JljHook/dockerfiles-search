FROM debian:stretch AS stage_build


RUN echo "## Update and install packages"
RUN apt-get update
RUN apt-get install -y --no-install-recommends python python-pip python-setuptools python-wheel curl
RUN echo "## Done"

RUN echo "## Installing CMake"
RUN curl https://cmake.org/files/v3.6/cmake-3.6.3-Linux-x86_64.sh --output cmake-3.6.3-Linux-x86_64.sh \
    &&  mkdir /opt/cmake \
    &&  printf "y\nn\n" | sh cmake-3.6.3-Linux-x86_64.sh --prefix=/opt/cmake > /dev/null \
    &&  ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake
RUN echo "## Done"

RUN echo "## Installing wasienv"
RUN curl https://raw.githubusercontent.com/wasienv/wasienv/master/install.sh | sh
RUN echo "## Done"
