# Pull base image.
FROM ubuntu:14.04

# Install Ubuntu Desktop.
RUN \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y build-essential && \
  apt-get install -y software-properties-common && \
  apt-get install -y byobu curl git htop man unzip vim wget && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y ubuntu-desktop tightvncserver

ADD root/.bashrc /root/.bashrc
ADD root/.gitconfig /root/.gitconfig
ADD root/.scripts /root/.scripts

ENV HOME /root
WORKDIR /opt

CMD ["bash"]
EXPOSE 5901