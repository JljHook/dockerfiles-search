FROM alpine

ARG BUILD_DATE
ARG BUILD_VERSION

LABEL org.opencontainers.image.authors="clemenko@docker.com" \
      org.opencontainers.image.source="https://github.com/clemenko/dockerfiles/tree/master/demo_flask" \
      org.opencontainers.image.created=$BUILD_DATE \
      org.opencontainers.image.title="clemenko/flask_demo" \
      org.opencontainers.image.description="The repository contains a simple flask application " \
      org.opencontainers.image.source=$BUILD_VERSION \
      org.stack="dmVyc2lvbjogIjMuNCIKc2VydmljZXM6CiAgYXBwOgogICAgaW1hZ2U6IGNsZW1lbmtvL2ZsYXNrX2RlbW8KICAgIGRlcGxveToKICAgICAgcmVwbGljYXM6IDQKICAgICAgdXBkYXRlX2NvbmZpZzoKICAgICAgICBwYXJhbGxlbGlzbTogMgogICAgICBsYWJlbHM6CiAgICAgICAgICBjb20uZG9ja2VyLmxiLm5ldHdvcms6IGZsYXNrX2t2CiAgICAgICAgICBjb20uZG9ja2VyLmxiLmhvc3RzOiBhcHAuZG9ja3IubGlmZQogICAgICAgICAgY29tLmRvY2tlci5sYi5wb3J0OiA1MDAwCiAgICBwb3J0czoKICAgICAgLSA1MDAwOjUwMDAKICAgIG5ldHdvcmtzOgogICAgICBkYjoKICAgICAga3Y6CgogIG1vbmdvOgogICAgaW1hZ2U6IG1vbmdvCiAgICBuZXR3b3JrczoKICAgICAgZGI6CgogIHJlZGlzOgogICAgaW1hZ2U6IHJlZGlzOmFscGluZQogICAgbmV0d29ya3M6CiAgICAgIGt2OgoKbmV0d29ya3M6CiAga3Y6CiAgZGI6Cg=="

RUN apk -U upgrade && apk add --no-cache python3 curl  &&\
    pip3 install --no-cache-dir --upgrade pip &&\
    pip3 install --no-cache-dir flask redis pymongo &&\
    rm -rf /var/cache/apk/*
    
WORKDIR /code
ADD . /code
EXPOSE 5000
HEALTHCHECK CMD curl -f http://localhost:5000/healthz || exit 1
CMD ["python3", "app.py"]
