FROM alpine:latest
MAINTAINER bluebu <bluebuwang@gmail.com>

#------------------------------------------------------------------------------
# Environment variables:
#------------------------------------------------------------------------------

RUN \
  apk --update --upgrade add \
      libsodium-dev \
      py-pip \
      privoxy \
      git \
  && rm /var/cache/apk/*

RUN pip install --upgrade git+https://github.com/shadowsocks/shadowsocks.git@master

ENV SERVER_ADDR= \
    SERVER_PORT=8899  \
    METHOD=aes-256-cfb \
    TIMEOUT=300 \
    PASSWORD=

#------------------------------------------------------------------------------
# Populate root file system:
#------------------------------------------------------------------------------

ADD rootfs /

#------------------------------------------------------------------------------
# Expose ports and entrypoint:
#------------------------------------------------------------------------------
EXPOSE 8118 7070

ENTRYPOINT ["/entrypoint.sh"]
