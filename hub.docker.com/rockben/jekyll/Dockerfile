FROM nginx:1.13.9-alpine

LABEL maintainer="Benson <qingwhat@gmail.com>"

ARG TZ='Asia/Shanghai'

ENV TZ ${TZ}

RUN apk upgrade --update \
    && apk add bash git \
    && rm -rf /usr/share/nginx/html \
    && git clone https://github.com/rockbenben/blog.git /usr/share/nginx/html \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && rm -rf /var/cache/apk/*

# ADD entrypoint.sh /entrypoint.sh
# ADD flush /usr/local/bin/flush

WORKDIR /usr/share/nginx/html

# CMD ["/entrypoint.sh"]
