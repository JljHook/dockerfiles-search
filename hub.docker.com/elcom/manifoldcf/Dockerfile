FROM adoptopenjdk:8-jre-hotspot
MAINTAINER LWB

ENV MANIFOLDCF_VERSION 2.14
ENV CIFS_VERSION 2.1.2

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --force-yes --no-install-recommends \
    wget curl ca-certificates \
    gzip && \
		rm -rf /var/lib/apt/lists/*

RUN wget http://apache.mirror.rafal.ca/manifoldcf/apache-manifoldcf-${MANIFOLDCF_VERSION}/apache-manifoldcf-${MANIFOLDCF_VERSION}-bin.tar.gz && \
    wget https://repo.maven.apache.org/maven2/eu/agno3/jcifs/jcifs-ng/${CIFS_VERSION}/jcifs-ng-${CIFS_VERSION}.jar && \
    tar -xzvf apache-manifoldcf-${MANIFOLDCF_VERSION}-bin.tar.gz && \
    cp -R apache-manifoldcf-${MANIFOLDCF_VERSION} /usr/share/manifoldcf && \
    cp jcifs-ng-${CIFS_VERSION}.jar /usr/share/manifoldcf/connector-lib-proprietary

RUN sed -i "s/<!--repositoryconnector name=\"Windows shares\" class=\"org.apache.manifoldcf.crawler.connectors.sharedrive.SharedDriveConnector\"\/-->/<repositoryconnector name=\"Windows shares\" class=\"org.apache.manifoldcf.crawler.connectors.sharedrive.SharedDriveConnector\"\/>/g" /usr/share/manifoldcf/connectors.xml && \
		sed -i "s/<property name=\"org.apache.manifoldcf.hsqldbdatabasepath\" value=\".\"\/>/<property name=\"org.apache.manifoldcf.hsqldbdatabasepath\" value=\"\/var\/manifoldcf\/\"\/>/g" /usr/share/manifoldcf/example/properties.xml

EXPOSE 8345

WORKDIR /usr/share/manifoldcf/example
VOLUME /var/manifoldcf

CMD ["java", "-jar", "start.jar"]
