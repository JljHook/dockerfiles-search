FROM uqlibrary/centos:191018

RUN \
 yum install -y cronie && \
 yum clean all

COPY etc/cron.d/timestamp-demo /etc/cron.d/timestamp-demo

CMD /usr/sbin/crond -n -x bit
