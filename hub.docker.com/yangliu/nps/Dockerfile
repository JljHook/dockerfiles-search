FROM alpine:3.9
MAINTAINER yangliu <istef.liu@gmail.com>

WORKDIR /
ENV NPS_VERSION 0.23.2
ENV NPS_RELEASE_URL https://github.com/cnlh/nps/releases/download/v0.23.1/linux_amd64_server.tar.gz

RUN set -x && \
	wget --no-check-certificate ${NPS_RELEASE_URL} && \ 
	tar xzf linux_amd64_server.tar.gz && \
    rm linux_amd64_server.tar.gz

VOLUME /nps/conf

CMD /nps/nps