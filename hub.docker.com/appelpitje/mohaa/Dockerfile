FROM debian:10-slim
LABEL maintainer "Appelpitje"
LABEL Description="MOHAA Server docker image" Version="0.1"

RUN dpkg --add-architecture i386
RUN apt-get update
RUN apt-get -y install curl ca-certificates openssl git tar bash sqlite fontconfig libstdc++5 libstdc++5:i386 wget bzip2

RUN adduser --disabled-password --home /home/container container

USER container
ENV  USER=container HOME=/home/container

WORKDIR /home/container

RUN wget -O MOHAA_MINI_SERVER.tar.bz2 https://appelpitje.be/download/MOHAA_MINI_SERVER.tar.bz2
RUN tar -xjf MOHAA_MINI_SERVER.tar.bz2
RUN rm -f MOHAA_MINI_SERVER.tar.bz2
RUN mv MOHAA_minisvr/* /home/container/
RUN rm -rf /home/container/MOHAA_minisvr

RUN chown -R container:container /home/container/*
RUN chmod 755 /home/container/*

WORKDIR /home/container

COPY ./entrypoint.sh /entrypoint.sh

CMD ["/bin/bash", "/entrypoint.sh"]

# RUN chmod 777 spearhead_lnxded && chmod 777 mainta/fgameded.so

# CMD ./spearhead_lnxded +set dedicated 1 +exec server.cfg

EXPOSE 12300/udp
EXPOSE 12203/udp
EXPOSE 12203/tcp