FROM archlinux/base:latest
MAINTAINER diodonfrost <diodon.frost@diodonfrost.me>

RUN pacman --noconfirm -Sy systemd procps; pacman --noconfirm -Scc ;\
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Install ansible
RUN pacman --noconfirm -Syu git ansible sudo openssh && pacman --noconfirm -Scc

VOLUME ["/sys/fs/cgroup"]

CMD ["/usr/lib/systemd/systemd"]
