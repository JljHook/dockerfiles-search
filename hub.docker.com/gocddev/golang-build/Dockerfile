FROM alpine:latest as gocd-agent-unzip

ARG UID=1000

RUN \
  apk --no-cache upgrade && \
  apk add --no-cache curl && \
  curl --fail --location --silent --show-error "https://download.gocd.org/binaries/19.8.0-9915/generic/go-agent-19.8.0-9915.zip" > /tmp/go-agent-19.8.0-9915.zip

RUN unzip /tmp/go-agent-19.8.0-9915.zip -d /
RUN mv /go-agent-19.8.0 /go-agent && chown -R ${UID}:0 /go-agent && chmod -R g=u /go-agent

FROM golang:1.13.0-buster
MAINTAINER ThoughtWorks, Inc. <support@thoughtworks.com>

LABEL gocd.version="19.8.0" \
  description="GoCD agent based on ubuntu version 18.04" \
  maintainer="ThoughtWorks, Inc. <support@thoughtworks.com>" \
  url="https://www.gocd.org" \
  gocd.full.version="19.8.0-9915" \
  gocd.git.sha="9ea99a72c338a132ae1ca83f363e16b2c95d920b"

ADD https://github.com/krallin/tini/releases/download/v0.18.0/tini-static-amd64 /usr/local/sbin/tini

# force encoding
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV GO_JAVA_HOME="/gocd-jre"

ARG UID=1000
ARG GID=1000

RUN \
# add mode and permissions for files we added above
  chmod 0755 /usr/local/sbin/tini && \
  chown root:root /usr/local/sbin/tini && \
# add our user and group first to make sure their IDs get assigned consistently,
# regardless of whatever dependencies get added
# add user to root group for gocd to work on openshift
  useradd -u ${UID} -g root -d /home/go -m go && \
  apt-get update && \
  apt-get install -y git subversion openssh-client bash zip unzip curl locales procps sysvinit-utils coreutils && \
  apt-get autoclean && \
  echo 'en_US.UTF-8 UTF-8' > /etc/locale.gen && /usr/sbin/locale-gen && \
  curl --fail --location --silent --show-error 'https://github.com/AdoptOpenJDK/openjdk12-binaries/releases/download/jdk-12.0.1%2B12/OpenJDK12U-jre_x64_linux_hotspot_12.0.1_12.tar.gz' --output /tmp/jre.tar.gz && \
  mkdir -p /gocd-jre && \
  tar -xf /tmp/jre.tar.gz -C /gocd-jre --strip 1 && \
  rm -rf /tmp/jre.tar.gz && \
  mkdir -p /go-agent /docker-entrypoint.d /gocd /godata

ADD docker-entrypoint.sh /


COPY --from=gocd-agent-unzip /go-agent /go-agent
# ensure that logs are printed to console output
COPY --chown=go:root agent-bootstrapper-logback-include.xml agent-launcher-logback-include.xml agent-logback-include.xml /go-agent/config/

RUN chown -R go:root /docker-entrypoint.d /gocd /godata /docker-entrypoint.sh \
    && chmod -R g=u /docker-entrypoint.d /gocd /godata /docker-entrypoint.sh


ENTRYPOINT ["/docker-entrypoint.sh"]

USER go
