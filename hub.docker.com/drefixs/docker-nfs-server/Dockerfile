FROM ubuntu
ENV DEBIAN_FRONTEND=noninteractive \
    EXT_OPTS=

RUN apt-get update -qq && apt-get install -y nfs-kernel-server runit netbase inotify-tools -qq
RUN mkdir -p /exports

RUN mkdir -p /etc/sv/nfs
ADD nfs.init /etc/sv/nfs/run
ADD nfs.stop /etc/sv/nfs/finish

ADD nfs_setup.sh /usr/local/bin/nfs_setup

VOLUME /exports

ENTRYPOINT ["/usr/local/bin/nfs_setup"]
