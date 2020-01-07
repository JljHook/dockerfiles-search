FROM elasticsearch:2.4.0

MAINTAINER Guillaume Goussard <guillaume.goussard@gmail.com>

ENV ES_COUCHBASE_USERNAME=Administrator \
    ES_COUCHBASE_PASSWORD=Administrator 

WORKDIR /usr/share/elasticsearch

RUN bin/plugin install -b https://github.com/couchbaselabs/elasticsearch-transport-couchbase/releases/download/2.${ELASTICSEARCH_VERSION}/elasticsearch-transport-couchbase-2.${ELASTICSEARCH_VERSION}.zip

COPY elasticsearch.yml config/elasticsearch.yml
COPY postinstall.sh /tmp/postinstall.sh
RUN bash /tmp/postinstall.sh
