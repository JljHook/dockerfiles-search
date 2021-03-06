FROM alpine:3.9 as build
WORKDIR /

RUN apk add --update --no-cache ca-certificates git

ENV VERSION=v2.14.1
ENV FILENAME=helm-${VERSION}-linux-amd64.tar.gz
ENV SHA256SUM=804f745e6884435ef1343f4de8940f9db64f935cd9a55ad3d9153d064b7f5896

RUN apk add --update -t deps curl tar gzip
RUN curl -L https://get.helm.sh/${FILENAME} > ${FILENAME} && \
    echo "${SHA256SUM}  ${FILENAME}" > helm_${VERSION}_SHA256SUMS && \
    sha256sum -cs helm_${VERSION}_SHA256SUMS && \
    tar zxv -C /tmp -f ${FILENAME} && \
    rm -f ${FILENAME}

# The image we keep
FROM alpine:3.9
WORKDIR /src

RUN apk add --update --no-cache ca-certificates curl bash

COPY --from=build /tmp/linux-amd64/helm /bin/helm
COPY --from=minio/mc /usr/bin/mc /bin/mc