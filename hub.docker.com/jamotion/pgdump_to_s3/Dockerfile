FROM alpine:3.3
MAINTAINER Renzo Meister <rm@jamotion.ch>

ENV TEMP_BACKUP_DIR /backuptemp
RUN mkdir $TEMP_BACKUP_DIR

RUN apk -Uv add python py-pip \
 && apk -UvX http://dl-4.alpinelinux.org/alpine/edge/main add postgresql \
 && apk -UvX http://dl-4.alpinelinux.org/alpine/edge/main add gzip \
 && apk -UvX http://dl-4.alpinelinux.org/alpine/edge/main add bash \
 && pip install --upgrade pip \
 && pip install awscli \
 && apk --purge -v del py-pip \
 && rm -rf /var/cache/apk/*
ADD ./dump_and_save /

ENTRYPOINT ["/dump_and_save"]
