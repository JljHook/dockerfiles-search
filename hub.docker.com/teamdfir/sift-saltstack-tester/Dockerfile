FROM ubuntu:18.04

RUN \
  apt-get update && \
  apt-get install -y wget gnupg && \
  wget -O - http://repo.saltstack.com/apt/ubuntu/18.04/amd64/2018.3/SALTSTACK-GPG-KEY.pub | apt-key add - && \
  echo "deb http://repo.saltstack.com/apt/ubuntu/18.04/amd64/2018.3 bionic main" | tee /etc/apt/sources.list.d/saltstack.list && \
  apt-get update && \
  apt-get install -y salt-minion

