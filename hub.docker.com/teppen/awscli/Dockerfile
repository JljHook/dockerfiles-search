FROM alpine
LABEL maintainer "tom@teppen.io"
RUN apk -v --update add \
        python \
        py-pip \
        git \
        && \
    pip install --no-cache-dir --upgrade awscli && \
    apk -v --purge del py-pip && \
    rm /var/cache/apk/*
