FROM debian
MAINTAINER David Personette <dperson@gmail.com>

# Install elasticsearch
RUN export DEBIAN_FRONTEND='noninteractive' && \
    export version='7.4.2' && \
    export shasum='64cc3e77f4271a5477c8c979fa48728d96890cad68b0daac6566f4d' && \
    export url='https://artifacts.elastic.co/downloads/elasticsearch' && \
    groupadd -r elasticsearch && \
    useradd -c 'Elasticsearch' -d /opt/elasticsearch -g elasticsearch -r \
                elasticsearch && \
    apt-get update -qq && \
    apt-get install -qqy --no-install-recommends ca-certificates curl procps \
                $(apt-get -s dist-upgrade|awk '/^Inst.*ecurity/ {print $2}') &&\
    file="elasticsearch-${version}-linux-x86_64.tar.gz" && \
    echo "downloading: $file ..." && \
    curl -LOSs ${url}/$file && \
    sha512sum $file | grep -q "$shasum" || \
    { echo "expected $shasum, got $(sha512sum $file)"; exit 13; } && \
    tar -xf $file -C /tmp && \
    mv /tmp/elasticsearch-* /opt/elasticsearch && \
    (echo '\nhttp.cors.enabled: true\n#http.cors.allow-origin:' && \
    echo 'http.host: 0.0.0.0') \
                >>/opt/elasticsearch/config/elasticsearch.yml && \
    sed -i 's/^\(-Xm[sx]\).*/\1512m/' /opt/elasticsearch/config/jvm.options && \
    chown -Rh elasticsearch. /opt/elasticsearch && \
    apt-get purge -qqy curl && \
    apt-get autoremove -qqy && apt-get clean -qqy && \
    rm -rf /tmp/* /var/lib/apt/lists/* $file
COPY elasticsearch.sh /usr/bin/

EXPOSE 9200 9300

VOLUME ["/opt/elasticsearch/config", "/opt/elasticsearch/data", \
            "/opt/elasticsearch/logs", "/opt/elasticsearch/plugins"]

ENTRYPOINT ["elasticsearch.sh"]