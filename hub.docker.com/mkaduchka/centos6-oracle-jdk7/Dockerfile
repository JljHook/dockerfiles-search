FROM centos:centos6

MAINTAINER Mikhail Kaduchka <mikhail.kaduchka@gmail.com>

RUN yum -y update && \
    yum -y install wget tzdata unzip && \
    wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u76-b13/jdk-7u76-linux-x64.rpm && \
    echo "993d0d6425951d2a3fe39d8ad2e550fc  jdk-7u76-linux-x64.rpm" >> MD5SUM && \
    md5sum -c MD5SUM && \
    rpm -Uvh jdk-7u76-linux-x64.rpm && \
    wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/tzupdater/2.0.0/tzupdater-2_0_0-2015a.zip && \
    unzip tzupdater-2_0_0-2015a.zip && \
    java -jar tzupdater-2.0.0-2015a/tzupdater.jar -u && \
    yum -y remove wget unzip && \
    yum clean all && \
    rm -fr jdk-7u76-linux-x64.rpm MD5SUM tzupdater-2_0_0-2015a.zip tzupdater-2.0.0-2015a
