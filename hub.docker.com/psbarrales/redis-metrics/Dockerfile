FROM golang:1.12-alpine as builder
WORKDIR /go/src/github.com/oliver006/redis_exporter/

ADD main.go /go/src/github.com/oliver006/redis_exporter/
ADD exporter /go/src/github.com/oliver006/redis_exporter/exporter
ADD vendor /go/src/github.com/oliver006/redis_exporter/vendor

ARG SHA1
ENV SHA1=$SHA1
ARG TAG
ENV TAG=$TAG

RUN apk --no-cache add ca-certificates
RUN BUILD_DATE=$(date +%F-%T) && CGO_ENABLED=0 GOOS=linux go build -o /redis_exporter \
    -ldflags  "-s -w -extldflags \"-static\" -X main.VERSION=$TAG -X main.COMMIT_SHA1=$SHA1 -X main.BUILD_DATE=$BUILD_DATE" .

RUN /redis_exporter -version

FROM alpine:3.9

COPY --from=builder /redis_exporter /redis_exporter
COPY --from=builder /etc/ssl/certs /etc/ssl/certs

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN addgroup -S redis && adduser -S -G redis redis

RUN apk add --no-cache \
    # grab su-exec for easy step-down from root
    'su-exec>=0.2' \
    # add tzdata for https://github.com/docker-library/redis/issues/138
    tzdata

ENV REDIS_VERSION 5.0.4
ENV REDIS_DOWNLOAD_URL http://download.redis.io/releases/redis-5.0.4.tar.gz
ENV REDIS_DOWNLOAD_SHA 3ce9ceff5a23f60913e1573f6dfcd4aa53b42d4a2789e28fa53ec2bd28c987dd

# for redis-sentinel see: http://redis.io/topics/sentinel
RUN set -ex; \
    \
    apk add --no-cache --virtual .build-deps \
    coreutils \
    gcc \
    linux-headers \
    make \
    musl-dev \
    ; \
    \
    wget -O redis.tar.gz "$REDIS_DOWNLOAD_URL"; \
    echo "$REDIS_DOWNLOAD_SHA *redis.tar.gz" | sha256sum -c -; \
    mkdir -p /usr/src/redis; \
    tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1; \
    rm redis.tar.gz; \
    \
    # disable Redis protected mode [1] as it is unnecessary in context of Docker
    # (ports are not automatically exposed when running inside Docker, but rather explicitly by specifying -p / -P)
    # [1]: https://github.com/antirez/redis/commit/edd4d555df57dc84265fdfb4ef59a4678832f6da
    grep -q '^#define CONFIG_DEFAULT_PROTECTED_MODE 1$' /usr/src/redis/src/server.h; \
    sed -ri 's!^(#define CONFIG_DEFAULT_PROTECTED_MODE) 1$!\1 0!' /usr/src/redis/src/server.h; \
    grep -q '^#define CONFIG_DEFAULT_PROTECTED_MODE 0$' /usr/src/redis/src/server.h; \
    # for future reference, we modify this directly in the source instead of just supplying a default configuration flag because apparently "if you specify any argument to redis-server, [it assumes] you are going to specify everything"
    # see also https://github.com/docker-library/redis/issues/4#issuecomment-50780840
    # (more exactly, this makes sure the default behavior of "save on SIGTERM" stays functional by default)
    \
    make -C /usr/src/redis -j "$(nproc)"; \
    make -C /usr/src/redis install; \
    \
    rm -r /usr/src/redis; \
    \
    runDeps="$( \
    scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
    | tr ',' '\n' \
    | sort -u \
    | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    )"; \
    apk add --virtual .redis-rundeps $runDeps; \
    apk del .build-deps; \
    \
    redis-server --version

RUN mkdir /data && chown redis:redis /data
VOLUME /data
WORKDIR /data

COPY inc/redis-entrypoint.sh /usr/local/bin/

EXPOSE 6379
EXPOSE 9121

ENTRYPOINT ["redis-entrypoint.sh"]

# CMD ["redis-server"]
