FROM debian:jessie

# Tooling

RUN apt-get update -y && \
apt-get install -y software-properties-common make bash curl apt-transport-https python python-pip && \
apt-get clean && \
pip --version

# Docker

RUN curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add - && \
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable" && \
apt-get update -y && \
apt-get install -y docker-ce && \
apt-get clean && \
docker --version

# Docker Compose

RUN curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose && \
chmod +x /usr/local/bin/docker-compose && \
docker-compose --version
