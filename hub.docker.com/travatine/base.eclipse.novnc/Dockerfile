FROM ubuntu:16.04

# Borrowed & modified from https://github.com/mccahill/docker-eclipse-novnc/blob/github/Dockerfile
# Updated to ubuntu 16.04


ENV DEBIAN_FRONTEND noninteractive
ENV HOME /root

# setup our Ubuntu sources (ADD breaks caching)
RUN echo "deb http://archive.ubuntu.com/ubuntu xenial main restricted universe multiverse\n\
deb http://archive.ubuntu.com/ubuntu xenial-updates main restricted universe multiverse\n\
deb http://archive.ubuntu.com/ubuntu xenial-backports main restricted universe multiverse\n\ 
deb http://security.ubuntu.com/ubuntu xenial-security main restricted universe multiverse \n\
"> /etc/apt/sources.list

RUN apt-get update \
    && apt-get upgrade -y

RUN apt-get install -y python-numpy
RUN apt-get install -y software-properties-common wget
RUN apt-get install -y --force-yes --no-install-recommends supervisor \
        openssh-server pwgen sudo vim-tiny \
        net-tools \
        lxde x11vnc xvfb \
        gtk2-engines-murrine ttf-ubuntu-font-family \
        firefox \
        xserver-xorg-video-dummy \
    && apt-get install -y desktop-file-utils \
    && apt-get autoclean \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /etc/startup.aux/
RUN echo "#Dummy" > /etc/startup.aux/00.sh
RUN chmod +x /etc/startup.aux/00.sh
RUN mkdir -p /etc/supervisor/conf.d
RUN rm /etc/supervisor/supervisord.conf

# create an ubuntu user
#PASS=`pwgen -c -n -1 10`
#PASS=ubuntu
#echo "User: ubuntu Pass: $PASS"
#RUN useradd --create-home --shell /bin/bash --user-group --groups adm,sudo ubuntu

# create an ubuntu user who cannot sudo
RUN useradd --create-home --shell /bin/bash --user-group ubuntu
RUN echo "ubuntu:badpassword" | chpasswd

ADD startup.sh /
ADD supervisord.conf.xorg /etc/supervisor/supervisord.conf
EXPOSE 6080
EXPOSE 5900
EXPOSE 22


WORKDIR /

# noVNC
ENV VNC_PASSWORD docker
ADD noVNC /noVNC/
# store a password for the VNC service
RUN mkdir /home/root
RUN mkdir /home/root/.vnc
RUN x11vnc -storepasswd $VNC_PASSWORD /home/root/.vnc/passwd
ADD xorg.conf /etc/X11/xorg.conf

ENTRYPOINT ["/startup.sh"]