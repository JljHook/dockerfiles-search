FROM opensuse:42.2
MAINTAINER diodonfrost <keres@protonmail.com>

ENV container docker

RUN zypper -n install systemd; zypper clean ; \
(cd /usr/lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /usr/lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /usr/lib/systemd/system/local-fs.target.wants/*; \
rm -f /usr/lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /usr/lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /usr/lib/systemd/system/basic.target.wants/*;\
rm -f /usr/lib/systemd/system/anaconda.target.wants/*;

RUN zypper -n install git wget sudo openssh && zypper clean

# Install puppet repo
RUN wget https://yum.puppet.com/puppet5/puppet5-release-sles-12.noarch.rpm; \
    rpm -ivh puppet5-release-sles-12.noarch.rpm

# Install puppet agent
RUN zypper -n --gpg-auto-import-keys install puppet-agent && zypper clean

VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/lib/systemd/systemd", "--system"]
