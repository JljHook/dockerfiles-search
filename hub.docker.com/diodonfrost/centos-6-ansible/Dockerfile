FROM centos:6
LABEL maintainer="diodonfrost <diodon.frost@diodonfrost.me>"

RUN yum -y install epel-release && yum clean all
RUN yum -y install git ansible sudo openssh-server openssh-clients unzip && yum clean all

RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts

VOLUME ["/sys/fs/cgroup"]

CMD ["/sbin/init"]
