FROM centos:7.2.1511
MAINTAINER Craig Willis (willis8@illinois.edu)

# Install dependencies
RUN yum install -y \
    cronie \
    git \
    java-1.8.0-openjdk-devel \
    nc \
    perl \
    postgresql \
    sha1sum \
    unzip \
    wget

ENV GLASSFISH_DOWNLOAD_SHA1 d1a103d06682eb08722fbc9a93089211befaa080
ENV GLASSFISH_DIRECTORY "/usr/local/glassfish4"
ENV HOST_DNS_ADDRESS "localhost"
ENV POSTGRES_DATABASE "dvndb"
ENV POSTGRES_USER "dvnapp"
ENV RSERVE_USER "rserve"
ENV RSERVE_PASSWORD "rserve"

# Install Glassfish 4.1
RUN cd /tmp \ 
    && wget http://dlc-cdn.sun.com/glassfish/4.1/release/glassfish-4.1.zip \ 
    && unzip glassfish-4.1.zip \
    && mv glassfish4 /usr/local \
    && cd /usr/local/glassfish4/glassfish/modules \
    && rm weld-osgi-bundle.jar \
    && wget http://central.maven.org/maven2/org/jboss/weld/weld-osgi-bundle/2.2.10.SP1/weld-osgi-bundle-2.2.10.SP1-glassfish4.jar \
    && echo "$GLASSFISH_DOWNLOAD_SHA1 weld-osgi-bundle-2.2.10.SP1-glassfish4.jar" | sha1sum -c - \
    && rm -rf /usr/local/glassfish4/glassfish/domains/domain1 \
    && rm /tmp/glassfish-4.1.zip 

# Install jq
RUN cd /tmp \  
    && wget https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64 \
    && mv jq-linux64 /usr/local/bin \
    && chmod +x /usr/local/bin/jq-linux64 \
    && ln -s /usr/local/bin/jq-linux64 /usr/local/bin/jq

# Customized persistence xml to avoid database recreation
RUN mkdir -p /tmp/WEB-INF/classes/META-INF/ 
COPY WEB-INF/classes/META-INF/persistence.xml /tmp/WEB-INF/classes/META-INF/

# Install DataVerse WAR
RUN  cd ~ \
    && wget https://github.com/IQSS/dataverse/releases/download/v4.2.3/dvinstall-4.2.3.zip \
    && unzip dvinstall-4.2.3.zip \
    && jar -uf dvinstall/dataverse.war -C /tmp/ /WEB-INF/classes/META-INF/persistence.xml

RUN ln -s /usr/local/glassfish4/glassfish/domains/domain1/logs /var/log/dataverse

# Install iRods iCommands
RUN  cd /tmp \
   && yum -y install epel-release \
   && yum -y install ftp://ftp.renci.org/pub/irods/releases/4.1.6/centos7/irods-icommands-4.1.6-centos7-x86_64.rpm 

COPY config-glassfish /root/dvinstall
COPY restart-glassfish /root/dvinstall
COPY config-dataverse /root/dvinstall

RUN cd /root/dvinstall && ./config-dataverse

VOLUME /usr/local/glassfish4/glassfish/domains/domain1/files
COPY ddl /root/dvinstall
COPY init-postgres /root/dvinstall
COPY init-glassfish /root/dvinstall
COPY init-dataverse /root/dvinstall
COPY setup-all.sh /root/dvinstall
COPY setup-irods.sh /root/dvinstall

COPY entrypoint.sh /
COPY Dockerfile /

EXPOSE      8080 

ENTRYPOINT ["/entrypoint.sh"]
CMD ["dataverse"]
