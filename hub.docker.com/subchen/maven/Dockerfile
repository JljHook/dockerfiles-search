FROM subchen/java:8
MAINTAINER Guoqiang Chen <subchen@gmail.com>

ENV MAVEN_VERSION 3.3.9
ENV MAVEN_HOME /usr/share/maven

RUN curl -fSL https://archive.apache.org/dist/maven/maven-${MAVEN_VERSION:0:1}/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz -o maven.tar.gz \
 && echo "516923b3955b6035ba6b0a5b031fbd8b maven.tar.gz" | md5sum -c \
 && mkdir -p $MAVEN_HOME \
 && tar xzf maven.tar.gz --strip-components=1 -C $MAVEN_HOME \
 && rm -rf maven.tar.gz \
 && ln -s $MAVEN_HOME/bin/mvn /usr/bin/mvn

VOLUME /root/.m2

