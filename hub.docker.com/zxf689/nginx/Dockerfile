FROM centos:7
MAINTAINER jay.yang "test@gmail.com"

RUN yum -y install httpd; yum clean all; systemctl enable httpd.service
EXPOSE 80
CMD ["/usr/sbin/init"]
