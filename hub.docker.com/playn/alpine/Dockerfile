FROM alpine:edge
RUN apk update && apk upgrade && apk add tzdata && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone && \
    apk del tzdata && rm -rf /var/cache/apk/*