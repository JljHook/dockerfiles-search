FROM alpine
#
# Include dist
ADD dist/ /root/dist/
#
# Setup env and apt
RUN sed -i 's/dl-cdn/dl-2/g' /etc/apk/repositories && \
    apk -U --no-cache add \
             aria2 \
             bash \
             bzip2 \
	     curl \
             libc6-compat \
             libzmq \
             nss \
             openjdk8-jre && \
#
# Get and install packages
    mkdir -p /etc/listbot && \
    cd /etc/listbot && \
    aria2c -s16 -x 16 https://raw.githubusercontent.com/dtag-dev-sec/listbot/master/cve.yaml.bz2 && \
    aria2c -s16 -x 16 https://raw.githubusercontent.com/dtag-dev-sec/listbot/master/iprep.yaml.bz2 && \
    bunzip2 *.bz2 && \
    cd /root/dist/ && \
    mkdir -p /usr/share/logstash/ && \
    aria2c -s 16 -x 16 https://artifacts.elastic.co/downloads/logstash/logstash-6.8.2.tar.gz && \
    tar xvfz logstash-6.8.2.tar.gz --strip-components=1 -C /usr/share/logstash/ && \
    /usr/share/logstash/bin/logstash-plugin install logstash-filter-translate && \
    /usr/share/logstash/bin/logstash-plugin install logstash-output-syslog && \
#
# Add and move files
    cd /root/dist/ && \
    cp update.sh /usr/bin/ && \
    chmod u+x /usr/bin/update.sh && \
    mkdir -p /etc/logstash/conf.d && \
    cp logstash.conf /etc/logstash/conf.d/ && \
    cp elasticsearch-template-es6x.json /usr/share/logstash/vendor/bundle/jruby/2.5.0/gems/logstash-output-elasticsearch-9.4.0-java/lib/logstash/outputs/elasticsearch/ && \
#
# Setup user, groups and configs
    addgroup -g 2000 logstash && \
    adduser -S -H -s /bin/bash -u 2000 -D -g 2000 logstash && \
    chown -R logstash:logstash /usr/share/logstash && \
    chown -R logstash:logstash /etc/listbot && \
    chmod 755 /usr/bin/update.sh && \
#
# Clean up
    rm -rf /root/* && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/apk/*
#
# Healthcheck
HEALTHCHECK --retries=10 CMD curl -s -XGET 'http://127.0.0.1:9600'
#
# Start logstash
#USER logstash:logstash
CMD update.sh && exec /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash.conf --config.reload.automatic --java-execution
