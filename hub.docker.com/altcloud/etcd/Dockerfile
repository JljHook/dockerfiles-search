FROM alt:p9

MAINTAINER alt-cloud

RUN apt-get update && \
    apt-get install -y etcd; \
    rm -f /var/cache/apt/archives/*.rpm \
          /var/cache/apt/*.bin \
          /var/lib/apt/lists/*.*

VOLUME /data

ENTRYPOINT ["/usr/sbin/etcd", "--data-dir", "/data"]
