FROM centos:centos7
MAINTAINER Martin
ENV container docker

# Install systemd -- See https://hub.docker.com/_/centos/
RUN yum -y update; yum clean all
RUN yum -y install systemd; yum clean all;\
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Install Ansible and other requirements.
RUN yum makecache fast && \
    yum -y install deltarpm initscripts && \
    yum -y update && \
    yum -y install \
      openssh-server \
      ansible \
      ansible-lint \
      policycoreutils-python \
      checkpolicy \
      sudo \
      crontabs \
      which && \
    yum clean all

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Install Ansible inventory file.
RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts

VOLUME ["/sys/fs/cgroup"]
CMD [ "ansible-playbook", "--version" ]
