##
## Small NodeJS 
##

FROM alpine:latest

MAINTAINER Francisco Correia <fcorreia.skywalker@gmail.com>

## Verify download
ENV NODE_SHA256     f8a5b7fe246f8358121e057f26411d3be9657a328b146d74abf483a956702868
ENV NODE_VERSION    4.8.3

RUN apk add --update openssl \
    &&  wget https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}.tar.gz \
            -O /tmp/node-src.tar.gz &&  mkdir -p /tmp/build  \
    &&  sha256sum /tmp/node-src.tar.gz | grep ${NODE_SHA256}    \
    &&  cd /tmp/build && tar -xzf /tmp/node-src.tar.gz \
    &&  rm /tmp/node-src.tar.gz && cd /tmp/build/node-v${NODE_VERSION} \
    &&  apk  add --no-cache curl make gcc g++ binutils-gold python linux-headers \
            paxctl libgcc libstdc++ gnupg \
    &&  cd /tmp/build/node-v${NODE_VERSION}  && ./configure && make && make install \
    &&  cd / && rm -rf /tmp/build \
    &&  apk del binutils binutils-gold binutils-libs ca-certificates curl db expat \
            g++ gcc gdbm gmp gnupg isl libassuan libatomic libattr libbz2 libc-dev \
            libcap libffi libgcrypt libgomp libgpg-error libksba libldap libsasl \
            libssh2 libuv-dev linux-headers make mpc1 mpfr3 musl-dev ncurses-libs \
            ncurses-terminfo ncurses-terminfo-base npth openssh openssh-client \
            openssh-sftp-server openssl paxctl pinentry pkgconf pkgconfig python \
            readline sqlite-libs \
    && rm -rf /var/cache/apk/*


# RUN apk add --update nodejs libgcc libstdc++ libuv

##ENTRYPOINT ["/usr/bin/node"]

CMD ["/usr/local/bin/node"]
