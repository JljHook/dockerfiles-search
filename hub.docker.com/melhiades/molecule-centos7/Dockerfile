FROM centos:7
LABEL maintainer="Roman I Pozdeev"
ENV container docker

# Install systemd -- See https://hub.docker.com/_/centos/
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Install requirements.
RUN yum makecache fast \
 && yum -y install deltarpm epel-release initscripts \
 && yum -y update \
 && yum -y install \
      sudo \
      which \
      xfsprogs \
      firewalld \
 && yum clean all

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Fix firewalld
RUN mkdir /lib/modules/$(uname -r) \
 && touch /lib/modules/$(uname -r)/modules.{builtin,order} \
 && for i in /sys/module/*; do echo kernel/${i##**/}.ko; done >> /lib/modules/$(uname -r)/modules.builtin \
 && depmod -a

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/sbin/init"]