FROM tor2/centos-source-base:4.0.0
MAINTAINER Kolla Project (https://launchpad.net/kolla)







RUN usermod --append --home /etc/telegraf --groups kolla telegraf \
    && mkdir -p /etc/telegraf \
    && chown -R 42448:42448 /etc/telegraf



RUN yum -y install telegraf && yum clean all

COPY telegraf_sudoers /etc/sudoers.d/kolla_telegraf_sudoers
COPY extend_start.sh /usr/local/bin/kolla_extend_start

RUN chmod 750 /etc/sudoers.d \
    && chmod 440 /etc/sudoers.d/kolla_telegraf_sudoers \
    && chmod 755 /usr/local/bin/kolla_extend_start




USER telegraf