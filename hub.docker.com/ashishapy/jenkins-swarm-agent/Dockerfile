FROM docker:1.12

MAINTAINER Ashish Pandey <ashishapy@gmailcom>

ENV SWARM_CLIENT_VERSION 2.2
ENV DOCKER_COMPOSE_VERSION 1.9.0
ENV COMMAND_OPTIONS ""

RUN adduser -G root -D jenkins
RUN apk --update add openjdk8-jre python py-pip git

RUN wget -q https://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/${SWARM_CLIENT_VERSION}/swarm-client-${SWARM_CLIENT_VERSION}-jar-with-dependencies.jar -P /home/jenkins/
RUN pip install docker-compose

COPY run.sh /run.sh
RUN chmod +x /run.sh

CMD ["/run.sh"]