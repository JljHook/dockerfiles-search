FROM dwolla/jenkins-agent-core:debian
LABEL org.label-schema.vcs-url="https://github.com/Dwolla/jenkins-agent-docker-nvm"
LABEL maintainer="dev+jenkins-nvm@dwolla.com"

ENV NVM_VERSION=v0.34.0 \
    NVM_DIR="${JENKINS_HOME}/.nvm"

COPY install.sh verify.sh /usr/local/bin/

USER root
RUN set -ex && \
    apt-get update && \
    apt-get install -y \
        libpng-dev \
        && \
    echo "dash dash/sh boolean false" | debconf-set-selections && \
    dpkg-reconfigure dash -f noninteractive && \
    apt-get clean

USER jenkins
RUN mkdir -p ${NVM_DIR} && \
    curl -L https://raw.githubusercontent.com/creationix/nvm/${NVM_VERSION}/install.sh | /bin/bash
