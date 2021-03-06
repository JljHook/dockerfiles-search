FROM debian:10 AS build

ARG LIBEVENT_VERSION="2.1.11-stable"
ARG LIBEVENT_CHECKSUM="a65bac6202ea8c5609fd5c7e480e6d25de467ea1917c08290c521752f147283d"

ARG MEMCACHED_VERSION="1.5.18"
ARG MEMCACHED_CHECKSUM="9ebdd6705b06da70eb6f68245686554b1cb689f00b9bff6e33672c1a0125ef88"

ADD https://github.com/libevent/libevent/releases/download/release-$LIBEVENT_VERSION/libevent-$LIBEVENT_VERSION.tar.gz /tmp/libevent.tar.gz
ADD http://www.memcached.org/files/memcached-$MEMCACHED_VERSION.tar.gz /tmp/memcached.tar.gz

RUN [ "$LIBEVENT_CHECKSUM" = "$(sha256sum /tmp/libevent.tar.gz | awk '{print $1}')" ] && \
    [ "$MEMCACHED_CHECKSUM" = "$(sha256sum /tmp/memcached.tar.gz | awk '{print $1}')" ] && \
    tar -C /tmp -xf /tmp/libevent.tar.gz && \
    tar -C /tmp -xf /tmp/memcached.tar.gz && \
    mv /tmp/memcached-$MEMCACHED_VERSION /tmp/memcached

RUN apt update && \
    apt install -y gcc make && \
    cd /tmp/libevent-$LIBEVENT_VERSION && \
      ./configure && \
      make && \
      make install && \
    cd /tmp/memcached && \
      ./configure && \
      make \
        CFLAGS="-fstack-protector-all" \
        LDFLAGS="-lpthread -z relro -z now" && \
    echo "nogroup:*:100:nobody" > /tmp/group && \
    echo "nobody:*:100:100:::" > /tmp/passwd


FROM scratch

COPY --from=build --chown=100:100 /tmp/memcached/memcached /
COPY --from=build --chown=100:100 /lib/x86_64-linux-gnu/libc.so.6 \
                                  /lib/x86_64-linux-gnu/libpthread.so.0 \
                                  /usr/local/lib/libevent-2.1.so.7 \
                                  /lib/x86_64-linux-gnu/
COPY --from=build --chown=100:100 /lib64/ld-linux-x86-64.so.2 /lib64/
COPY --from=build --chown=100:100 /tmp/group \
                                  /tmp/passwd \
                                  /etc/

USER 100:100
EXPOSE 11211/tcp
ENTRYPOINT ["/memcached"]
