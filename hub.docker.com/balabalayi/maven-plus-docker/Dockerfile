FROM maven:3-jdk-8
MAINTAINER BalaBalaYi

RUN apt-get update -y
RUN apt-get install -y sudo apt-transport-https ca-certificates curl
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
RUN echo "deb https://apt.dockerproject.org/repo debian-jessie main" > /etc/apt/sources.list.d/docker.list
RUN apt-get update -y
RUN apt-get install -y docker-engine=17.03.1~ce-0~debian-jessie
