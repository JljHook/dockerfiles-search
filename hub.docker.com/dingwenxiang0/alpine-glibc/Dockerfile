FROM alpine:3.7

MAINTAINER saruman 624380857@qq.com

ENV LANG=en_US.UTF-8

ARG ALPINE_GLIBC_BASE_URL=https://github.com/sgerrand/alpine-pkg-glibc/releases/download
ARG ALPINE_GLIBC_RSA_PUB_URL=https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub
ARG ALPINE_GLIBC_PACKAGE_VERSION=2.27-r0
ARG ALPINE_GLIBC_BASE_PACKAGE_FILENAME=glibc-$ALPINE_GLIBC_PACKAGE_VERSION.apk
ARG ALPINE_GLIBC_BIN_PACKAGE_FILENAME=glibc-bin-$ALPINE_GLIBC_PACKAGE_VERSION.apk
ARG ALPINE_GLIBC_I18N_PACKAGE_FILENAME=glibc-i18n-$ALPINE_GLIBC_PACKAGE_VERSION.apk

RUN apk add --no-cache --virtual=.build-dependencies wget ca-certificates \
    && wget "$ALPINE_GLIBC_RSA_PUB_URL" -O "/etc/apk/keys/sgerrand.rsa.pub" \
    && wget \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_I18N_PACKAGE_FILENAME" \
    && apk add --no-cache \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_I18N_PACKAGE_FILENAME" \
    && rm "/etc/apk/keys/sgerrand.rsa.pub" \
    && /usr/glibc-compat/bin/localedef --force --inputfile POSIX --charmap UTF-8 "$LANG" || true \
    && echo "export LANG=$LANG" > /etc/profile.d/locale.sh \
    && apk del glibc-i18n \
    && rm "/root/.wget-hsts" \
    && apk del .build-dependencies \
    && rm \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_I18N_PACKAGE_FILENAME"
