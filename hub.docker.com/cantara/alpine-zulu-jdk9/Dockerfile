FROM frolvlad/alpine-glibc:alpine-3.5_glibc-2.25

MAINTAINER josh@grahamis.com

ENV JAVA_HOME /usr/local/java
ENV JRE ${JAVA_HOME}/jre
ENV JAVA_OPTS=-Djava.awt.headless=true PATH=${PATH}:${JRE}/bin:${JAVA_HOME}/bin
ENV ENV=/etc/shinit.sh

COPY shinit.sh /etc/

WORKDIR /tmp

# If release changes, the checksum and URL need to be updated
# See http://www.azulsystems.com/products/zulu/downloads#Linux
#
# Replace duplicate files in JDK bin with links to JRE bin
RUN \
  echo ipv6 >> /etc/modules && \
  echo 'http://dl-cdn.alpinelinux.org/alpine/v3.5/main/' > /etc/apk/repositories && \
  apk add --no-cache --virtual=build-dependencies ca-certificates wget && \
  sed -i -e 's#:/bin/[^:].*$#:/sbin/nologin#' /etc/passwd && \
  chmod a=rx /etc/shinit.sh && \
  checksum="1931ed3beedee0b16fb7fd37e069b162" && \
  url="http://cdn.azul.com/zulu/bin/zulu8.21.0.1-jdk8.0.131-linux_x64.tar.gz" && \
  referer="http://zulu.org/download/" && \
  wget --referer "${referer}" "${url}" && \
  md5=$(md5sum *.tar.gz | cut -f1 -d' ') && \
  if [ ${checksum} != ${md5} ]; then \
    echo "[FATAL] File md5 ${md5} doesn't match published checksum ${checksum}. Exiting." >&2 && \
    exit 1; \
  fi && \
  rm -f APKINDEX.* && \
  tar -xzf *.tar.gz && \
  rm -f *.tar.gz && \
  mkdir -p $(dirname ${JAVA_HOME}) && \
  mv * ${JAVA_HOME} && \
  cd ${JAVA_HOME} && \
#  rm -rf *.zip demo man sample && \
  for ff in ${JAVA_HOME}/bin/*; do f=$(basename $ff); if [ -e ${JRE}/bin/$f ]; then ln -snf ${JRE}/bin/$f $ff; fi; done && \
  chmod a+w ${JRE}/lib ${JRE}/lib/net.properties && \
  apk del ca-certificates openssl wget && \
  rm -rf /tmp/* /var/cache/apk/* && \
  java -version

RUN wget http://cdn.azul.com/zulu/bin/zulu9.0.0.15-jdk9.0.0-linux_x64.tar.gz
RUN tar -xzf *.tar.gz
RUN mv * ${JAVA_HOME}
RUN java -version

WORKDIR /root
