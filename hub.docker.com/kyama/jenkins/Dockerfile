#
# jenkins with gnu make, ruby, scala
#
# @date 2015.02.15
#
FROM java:openjdk-7u65-jdk

RUN apt-get update && apt-get install -y wget git curl zip

# BEGIN:original
RUN apt-get update && apt-get install -y build-essential

RUN apt-get update && apt-get install -y libjansi-java
RUN wget http://www.scala-lang.org/files/archive/scala-2.10.3.deb 
RUN dpkg -i scala-2.10.3.deb
RUN rm -f scala-2.10.3.deb

RUN apt-get update && apt-get install -y r-base r-base-dev texlive-full
RUN apt-get update && apt-get install -y --force-yes openssl libssl-dev && rm -rf /var/lib/apt/lists/*
RUN \
  apt-get update && \
  apt-get install -y ruby ruby-dev rubygems && \
  rm -rf /var/lib/apt/lists/* 
RUN gem install bundler
RUN apt-get update && apt-get install -y sqlite3 libsqlite3-dev
# END:original

ENV JENKINS_HOME /var/jenkins_home

# Jenkins is ran with user `jenkins`, uid = 1000
# If you bind mount a volume from host/vloume from a data container, 
# ensure you use same uid
RUN useradd -d "$JENKINS_HOME" -u 1000 -m -s /bin/bash jenkins

# Jenkins home directoy is a volume, so configuration and build history 
# can be persisted and survive image upgrades
VOLUME /var/jenkins_home

# `/usr/share/jenkins/ref/` contains all reference configuration we want 
# to set on a fresh new installation. Use it to bundle additional plugins 
# or config file with your custom jenkins Docker image.
RUN mkdir -p /usr/share/jenkins/ref/init.groovy.d


COPY init.groovy /usr/share/jenkins/ref/init.groovy.d/tcp-slave-angent-port.groovy

#ENV JENKINS_VERSION %%JENKINS_VERSION%%
ENV JENKINS_VERSION 1.596

# could use ADD but this one does not check Last-Modified header 
# see https://github.com/docker/docker/issues/8331
RUN curl -L http://mirrors.jenkins-ci.org/war/1.596/jenkins.war -o /usr/share/jenkins/jenkins.war

ENV JENKINS_UC https://updates.jenkins-ci.org
RUN chown -R jenkins "$JENKINS_HOME" /usr/share/jenkins/ref

# for main web interface:
EXPOSE 8080

# will be used by attached slave agents:
EXPOSE 50000

USER jenkins

COPY jenkins.sh /usr/local/bin/jenkins.sh
ENTRYPOINT ["/usr/local/bin/jenkins.sh"]

# from a derived Dockerfile, can use `RUN plugin.sh active.txt` to setup /usr/share/jenkins/ref/plugins from a support bundle
COPY plugins.sh /usr/local/bin/plugins.sh
