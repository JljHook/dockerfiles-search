FROM openjdk:8-jre-slim-stretch

ENV MINECRAFT_DIR=/opt/minecraft \
    MINECRAFT_USER=minecraft \
    MINECRAFT_GROUP=minecraft \
    MINECRAFT_SERVER_JAR=minecraft-server.jar \
    JMXREMOTE_PORT=3000 \
    JMXREMOTE_HOSTNAME=localhost \
    GOSU_VERSION=1.11 \
    MCRCON_VERSION=0.0.5

RUN apt-get update && \
    apt-get install -y rsync wget && \
    wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64" && \
    chmod +x /usr/local/bin/gosu && \
    wget -O mcrcon.tar.gz https://github.com/Tiiffi/mcrcon/releases/download/v${MCRCON_VERSION}/mcrcon-${MCRCON_VERSION}-linux-x86-64.tar.gz && \
    tar xf mcrcon.tar.gz && \
    mv mcrcon /opt/mcrcon && \
    chmod +x /opt/mcrcon && \
    apt-get remove -y wget && \
    rm -rf /var/lib/apt/lists/*

RUN groupadd -g 999 ${MINECRAFT_USER} && \
    useradd -d ${MINECRAFT_DIR} -m -u 999 -g 999 ${MINECRAFT_GROUP} && \
    mkdir -p /docker-entrypoint.d

COPY ./mcrcon.sh /usr/local/bin/mcrcon
COPY ./docker-entrypoint.sh /docker-entrypoint.sh

EXPOSE 25565 ${JMXREMOTE_PORT}

VOLUME ${MINECRAFT_DIR}

WORKDIR ${MINECRAFT_DIR}

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["server"]