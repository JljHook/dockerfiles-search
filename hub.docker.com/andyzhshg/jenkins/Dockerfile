FROM jenkins:2.60.3
MAINTAINER andyzhshg <andyzhshg@gmail.com>

USER root
RUN apt-get update \
  && apt-get install -y sudo \
  && rm -rf /var/lib/apt/lists/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers

USER jenkins
