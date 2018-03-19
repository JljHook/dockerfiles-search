FROM alpine:3.7
LABEL maintainer="devops@sparklane.fr"

RUN apk -Uv add su-exec coreutils perl perl-switch perl-datetime perl-lwp-protocol-https perl-digest-sha1 \
 && apk -Uv --allow-untrusted -X http://dl-3.alpinelinux.org/alpine/edge/testing add perl-sys-syslog \
 && wget -O /tmp/aws-scripts-mon.zip http://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.1.zip \
 && mkdir /opt \
 && unzip -d /opt /tmp/aws-scripts-mon.zip \
 && chmod +x /opt/aws-scripts-mon/*.pl \
 && rm -rf /var/cache/apk/* /tmp/*

COPY run.sh /
ENTRYPOINT [ "sh", "/run.sh" ]
