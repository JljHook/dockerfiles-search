FROM ipman1971/java7_base
MAINTAINER ipman1971
 
ENV DEBIAN_FRONTEND noninteractive
 
RUN mkdir /data
 
RUN \
wget https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.1.1/elasticsearch-2.1.1.tar.gz && \
tar xvzf elasticsearch-2.1.1.tar.gz && \
rm -f elasticsearch-2.1.1.tar.gz && \
chown -R elkuser:elkuser elasticsearch-2.1.1
 
# elasticsearch.yml and Dockerfile must be on same location
ADD elasticsearch.yml /home/elkuser/elasticsearch-2.1.1/config/elasticsearch.yml
ENTRYPOINT elasticsearch-2.1.1/bin/elasticsearch
