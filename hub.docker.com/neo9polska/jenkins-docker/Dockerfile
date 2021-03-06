FROM neo9polska/java:8
MAINTAINER Neo9 Polska <labs@neo9.pl>
ENV REFRESHED_AT 2015-08-12

USER root

# general dependencies
RUN apt-get update -qq && apt-get install -qqy apt-transport-https iptables ca-certificates git-core

# install docker
RUN curl https://get.docker.com/gpg | apt-key add -
RUN echo deb http://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list
RUN apt-get update -qq && apt-get install -qqy  lxc lxc-docker

# install docker compose
ENV COMPOSE_VERSION 1.4.0

RUN curl -L https://github.com/docker/compose/releases/download/$COMPOSE_VERSION/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# install jenkins
ENV JENKINS_HOME /opt/jenkins/data
ENV JENKINS_MIRROR http://mirrors.jenkins-ci.org

RUN mkdir -p $JENKINS_HOME/plugins
RUN curl -sf -o /opt/jenkins/jenkins.war -L $JENKINS_MIRROR/war-stable/latest/jenkins.war

# jenkins plugins
RUN for plugin in greenballs scm-api git-client git ws-cleanup ;\
    do curl -sf -o $JENKINS_HOME/plugins/${plugin}.hpi \
       -L $JENKINS_MIRROR/plugins/${plugin}/latest/${plugin}.hpi ; done

COPY scripts/*.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/*.sh

VOLUME /var/lib/docker

EXPOSE 8080

ENTRYPOINT [ "/usr/local/bin/dockerjenkins.sh" ]
