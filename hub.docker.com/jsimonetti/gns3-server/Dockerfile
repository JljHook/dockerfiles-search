FROM jsimonetti/alpine-edge:latest

# Install the magic wrapper.
ADD ./start.sh /start.sh
ADD ./config.ini /config.ini

RUN sed -n "s/main/testing/p" /etc/apk/repositories >> /etc/apk/repositories && \
    mkdir /data && \
    apk add --no-cache dnsmasq cpulimit docker dynamips gns3-server qemu-img qemu-system-x86_64 ubridge vpcs iouyap wget && \
    pip3 install idna jsonschema==2.6.0 && \
    wget -O /usr/bin/vpcs https://github.com/GNS3/vpcs/releases/download/v0.8beta1/vpcs

CMD [ "/start.sh" ]

WORKDIR /data

VOLUME ["/data"]

