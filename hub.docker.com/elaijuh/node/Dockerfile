FROM alpine:edge
MAINTAINER hjl <jiale@implustech.com>

RUN apk update && apk upgrade && \
    apk add --no-cache bash git nodejs-current && \
        echo -e 'http://dl-cdn.alpinelinux.org/alpine/edge/main\nhttp://dl-cdn.alpinelinux.org/alpine/edge/community\nhttp://dl-cdn.alpinelinux.org/alpine/edge/testing' > /etc/apk/repositories && \
	    apk add --no-cache yarn
