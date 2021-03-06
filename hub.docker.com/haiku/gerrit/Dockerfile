FROM openjdk:8-jre-alpine

MAINTAINER Haiku, Inc <haiku-inc@gmail.com>

# Overridable defaults
ENV GERRIT_HOME /var/gerrit
ENV GERRIT_SITE ${GERRIT_HOME}/review_site
ENV GERRIT_WAR ${GERRIT_HOME}/gerrit.war
ENV GERRIT_VERSION 2.16.2
ENV GERRIT_USER gerrit2
ENV GERRIT_INIT_ARGS "--install-plugin=download-commands"

# Add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN adduser -D -h "${GERRIT_HOME}" -g "Gerrit User" -s /sbin/nologin "${GERRIT_USER}"

RUN set -x \
    && apk add --update --no-cache git openssh openssl bash perl perl-cgi git-gitweb curl su-exec procmail python py2-pip

# Build python twisted
RUN set -x \
    && apk add --no-cache --virtual=build-dependencies gcc g++ make python-dev
RUN pip install twisted
RUN apk del --purge build-dependencies

RUN mkdir /docker-entrypoint-init.d

#Download gerrit.war
RUN curl -fSsL https://gerrit-releases.storage.googleapis.com/gerrit-${GERRIT_VERSION}.war -o $GERRIT_WAR
#Only for local test
#COPY gerrit-${GERRIT_VERSION}.war $GERRIT_WAR

ENV GERRITFORGE_URL=https://gerrit-ci.gerritforge.com
ENV GERRITFORGE_ARTIFACT_DIR=lastSuccessfulBuild/artifact/bazel-genfiles/plugins

# Download "stable" plugins
ENV PLUGINS="delete-project events-log admin-console webhooks"
ENV PLUGIN_VERSION=bazel-stable-2.16
RUN for i in ${PLUGINS}; do \
	curl -fSsL ${GERRITFORGE_URL}/job/plugin-$i-${PLUGIN_VERSION}/${GERRITFORGE_ARTIFACT_DIR}/$i/$i.jar \
		-o ${GERRIT_HOME}/$i.jar; \
	done;

# Download "master-stable" plugins
ENV MASTER_PLUGINS="avatars-gravatar gitiles"
ENV MASTER_PLUGIN_VERSION=bazel-master-stable-2.16

RUN for i in ${MASTER_PLUGINS}; do \
	curl -fSsL ${GERRITFORGE_URL}/job/plugin-$i-${MASTER_PLUGIN_VERSION}/${GERRITFORGE_ARTIFACT_DIR}/$i/$i.jar \
		-o ${GERRIT_HOME}/$i.jar; \
	done;

#oauth2 plugin
ENV GERRIT_OAUTH_VERSION 2.16.1
RUN curl -fSsL \
    https://github.com/davido/gerrit-oauth-provider/releases/download/v${GERRIT_OAUTH_VERSION}/gerrit-oauth-provider.jar \
    -o ${GERRIT_HOME}/gerrit-oauth-provider.jar

# Ensure the entrypoint scripts are in a fixed location
COPY gerrit-entrypoint.sh /
COPY gerrit-start.sh /
RUN chmod +x /gerrit*.sh

#A directory has to be created before a volume is mounted to it.
#So gerrit user can own this directory.
RUN su-exec ${GERRIT_USER} mkdir -p $GERRIT_SITE

#Gerrit site directory is a volume, so configuration and repositories
#can be persisted and survive image upgrades.
VOLUME $GERRIT_SITE

ENTRYPOINT ["/gerrit-entrypoint.sh"]

EXPOSE 8080 29418

CMD ["/gerrit-start.sh"]
