FROM docker.io/openshift/jenkins-2-centos7
USER root
RUN yum -y install http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-2.noarch.rpm && \
    yum -y install git
USER 1000