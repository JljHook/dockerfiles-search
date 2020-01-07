FROM jenkins
USER root

RUN apt-get update && apt-get -y install apt-utils libsasl2-modules libevent-dev 
RUN curl -s http://repos.mesosphere.com/debian/pool/main/m/mesos/mesos_1.1.0-2.0.107.debian81_amd64.deb > mesos.deb
RUN dpkg -x mesos.deb /tmp/mesos-pkg && rm mesos.deb

RUN cp /tmp/mesos-pkg/usr/lib/libmesos* /usr/lib/
RUN mkdir -p /var/jenkins_backups && chown jenkins:jenkins /var/jenkins_backups

ENV MESOS_NATIVE_JAVA_LIBRARY=/usr/lib/libmesos.so
ENV MESOS_NATIVE_LIBRARY=$MESOS_NATIVE_JAVA_LIBRARY

ADD plugins.txt .
RUN plugins.sh plugins.txt

#expose a backups volume for easy retrieval later
VOLUME ["/var/jenkins_home","/var/jenkins_backups"]

#cleanup
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
