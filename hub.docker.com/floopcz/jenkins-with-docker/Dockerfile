# Jenkins with docker.
# Usage example:
# docker build -t floopcz/jenkins-with-docker:latest -f jenkins-with-docker .
# docker run -p 8080:8080 -p 50000:50000 \
#            -v jenkins_home:/var/jenkins_home \
#            -v /var/run/docker.sock:/var/run/docker.sock \
#            --name jenkins --detach --restart unless-stopped \
#            floopcz/jenkins-with-docker:latest

FROM jenkins/jenkins:lts

USER root

RUN apt-get update
RUN apt-get install -y \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg2 \
      software-properties-common \
    && apt-get clean -y

RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/debian \
      $(lsb_release -cs) \
      stable"

RUN apt-get update \
    && apt-get install -y docker-ce \
    && apt-get clean -y

RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
USER jenkins
