FROM ubuntu:18.04
LABEL  maintainer="LK"

COPY AutoSSH /usr/bin/AutoSSH

RUN \
#==================================================
# Executable
#==================================================
  chmod 777 /usr/bin/AutoSSH &&\
#==================================================
# Work space
#==================================================
  #mkdir /code &&\

#==================================================
# Ubuntu chinese source
#==================================================
  #sed -i "s@http://.*archive.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list && \
  #sed -i "s@http://.*security.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list && \

#==================================================
# Python3.6
#==================================================
  apt-get update &&\
  apt-get -qqy --no-install-recommends install \
    python3.6 \
    python3-pip \
    python3-setuptools \ 
    curl &&\

#==================================================
# Flask
#==================================================
  #pip3 install flask &&\
  #pip3 install flask-restful &&\
  pip3 install wheel &&\
  pip3 install flask_restplus &&\
  pip3 install requests &&\

#==================================================
# apt clean
#==================================================
  apt-get clean && rm -rf /var/lib/apt/lists/* \
