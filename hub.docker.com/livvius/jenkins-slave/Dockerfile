FROM cloudbees/jnlp-slave-with-java-build-tools

USER root

RUN apt-get -y install \
  apt-transport-https \
  ca-certificates \
  curl \
  software-properties-common

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

RUN add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"

RUN apt-get update

RUN apt-get -y install docker-ce
