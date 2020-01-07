FROM alpine:latest

ENV CLI_VERSION=1.16.29
ENV LINT_VERSION=0.7.4

RUN mkdir -p /aws && \
    apk -Uuv add groff jq less python py-pip && \
    pip install --upgrade pip && \
    pip install awscli==$CLI_VERSION && \
    pip install cfn-lint==$LINT_VERSION

WORKDIR /aws
