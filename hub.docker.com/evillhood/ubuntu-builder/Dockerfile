FROM ubuntu:19.04 as build
MAINTAINER EvillHood
ENV DEBIAN_FRONTEND=noninteractive

# install packages
#############

# Build tools
#############
RUN apt-get update && apt-get install -y build-essential cmake git
RUN apt-get install -y clang-tidy clazy clang

# libraries
#############
RUN apt-get install -y libboost-locale-dev libboost-regex-dev libboost-filesystem-dev libboost-log-dev libboost-thread-dev libboost-program-options-dev
RUN apt-get install -y qtbase5-dev gettext qttools5-dev-tools libqt5svg5-dev
RUN apt-get install -y qt5-default

# Add radio components
#############
RUN apt-get install -y libqwt-qt5-dev gnuradio-dev libuhd-dev

# Install vnc, xvfb in order to create a 'fake' display
#############
RUN apt-get install -y x11vnc xvfb twm

# map /source to host source data path (used to )
VOLUME /source

# map /data to host defined data path (used to store data from app)
VOLUME /data

# run
CMD ["/bin/bash"]
