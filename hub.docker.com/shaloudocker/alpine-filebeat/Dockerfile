FROM registry.cn-hangzhou.aliyuncs.com/shalousun/alpine-zh:3.8

MAINTAINER yusun <836575280@qq.com>

ENV FILEBEAT_VERSION=6.4.2

# Install filebeat

COPY ./config/filebeat.yml /

RUN apk add --update-cache curl bash libc6-compat && \
    rm -rf /var/cache/apk/* && \
    curl https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-${FILEBEAT_VERSION}-linux-x86_64.tar.gz -o /filebeat.tar.gz && \
    tar xzvf filebeat.tar.gz && \
    rm filebeat.tar.gz && \
    mv filebeat-${FILEBEAT_VERSION}-linux-x86_64 filebeat && \
    cd filebeat && \
    cp filebeat /usr/bin && \
    rm -rf /filebeat/filebeat.yml && \
    cp /filebeat.yml /filebeat/ && \
    ls -ltr /filebeat && \
    cat /filebeat/filebeat.yml

VOLUME /filebeat/data

WORKDIR /filebeat/
CMD ["./filebeat","-e","-c", "filebeat.yml"]