FROM datadog/docker-dd-agent

RUN echo 'deb http://httpredir.debian.org/debian jessie-backports main' > /etc/apt/sources.list.d/jessie-backports.list
RUN apt-get update && apt-get install -y wget unzip

# -----

ENV JAVA_VERSION 8u45
ENV JAVA_DEBIAN_VERSION 8u45-b14-2~bpo8+2

# see https://bugs.debian.org/775775
# and https://github.com/docker-library/java/issues/19#issuecomment-70546872
ENV CA_CERTIFICATES_JAVA_VERSION 20140324

RUN set -x \
	&& apt-get update \
	&& apt-get install -y \
		openjdk-8-jdk="$JAVA_DEBIAN_VERSION" \
		ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION"

# see CA_CERTIFICATES_JAVA_VERSION notes above
RUN /var/lib/dpkg/info/ca-certificates-java.postinst configure

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


### https://registry.hub.docker.com/u/datadog/docker-dd-agent/
### Datadog
##### jmx
ADD conf.d/jmx.yaml /etc/dd-agent/conf.d/jmx.yaml
