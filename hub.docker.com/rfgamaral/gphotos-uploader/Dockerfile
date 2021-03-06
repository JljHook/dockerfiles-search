ARG ALPINE_VERSION="3.10"
ARG S6_OVERLAY_VERSION="1.22.1.0"

FROM golang:1.11-alpine${ALPINE_VERSION} AS builder

ENV QEMU_USER_STATIC_DOWNLOAD_URL https://github.com/multiarch/qemu-user-static/releases/download
ENV QEMU_USER_STATIC_TAGS_API https://api.github.com/repos/multiarch/qemu-user-static/tags

RUN \
    apk update && \
    apk add --no-cache --virtual build-dependencies \
        curl \
        g++ \
        git && \
    git clone https://github.com/rfgamaral/gphotos-uploader-cli.git --branch docker && \
    git clone https://github.com/rfgamaral/oauth2-noserver.git --branch docker && \
    cd gphotos-uploader-cli && \
    sed -i "s/~\/\.config\/gphotos-uploader-cli/\/config/" cmd/root.go && \
    sed -i "s/~\/\.config\/gphotos-uploader-cli/\/config/" config/config.go && \
    go generate && \
    cp /go/oauth2-noserver/oauth2ns.go /go/pkg/mod/github.com/nmrshll/oauth2-noserver@v0.0.0-20190221200101-9bf017bef639 && \
    GOOS=linux GOARCH=arm GOARM=7 go build -ldflags='-w -s' -o /go/bin/gphotos-uploader-cli && \
    export QEMU_USER_STATIC_LATEST_TAG=$(curl -s ${QEMU_USER_STATIC_TAGS_API} \
        | grep 'name.*v[0-9]' | head -n 1 | cut -d '"' -f 4) && \
    curl -SL "${QEMU_USER_STATIC_DOWNLOAD_URL}/${QEMU_USER_STATIC_LATEST_TAG}/x86_64_qemu-arm-static.tar.gz" \
        | tar xzv --directory /go/bin && \
    apk del build-dependencies

FROM arm32v7/alpine:${ALPINE_VERSION}

LABEL maintainer="master@ricardoamaral.net"

ARG BUILD_DATE
ARG S6_OVERLAY_VERSION
ARG VCS_REF

LABEL \
    org.label-schema.build-date="${BUILD_DATE}" \
    org.label-schema.description="Mass upload media folders to your Google Photos account with this Docker image." \
    org.label-schema.name="rfgamaral/gphotos-uploader" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.vcs-ref="${VCS_REF}" \
    org.label-schema.vcs-url="https://github.com/rfgamaral/docker-gphotos-uploader.git"

ENV GPU_SCHEDULE="0 */8 * * *"

COPY --from=builder /go/bin/qemu-arm-static /usr/bin/

ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-arm.tar.gz /tmp/

RUN \
    apk update && \
    apk add --no-cache \
        curl && \
    tar xzf /tmp/s6-overlay-arm.tar.gz -C / && \
    rm -rf /tmp/*

COPY --from=builder /go/bin/gphotos-uploader-cli /usr/local/bin/

COPY rootfs/ /

VOLUME ["/config", "/photos"]

ENTRYPOINT ["/init"]
