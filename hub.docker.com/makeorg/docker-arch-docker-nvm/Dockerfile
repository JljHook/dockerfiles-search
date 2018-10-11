FROM makeorg/docker-arch-docker:latest

MAINTAINER amine.zaghdoudi@gmail.com

RUN \
  touch /root/.profile && \
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash && \
  . /root/.profile && nvm install 10.11.0 && nvm alias default node

RUN pacman -Sy --noconfirm  && pacman -S archlinux-keyring --noconfirm && pacman -S yarn --noconfirm
ENV LANG=en_US.UTF-8
CMD ["/usr/bin/bash"]
