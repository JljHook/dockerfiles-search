FROM openjdk:jre

ENV CEREBRO_VERSION 0.8.1
RUN cd /opt/ \
    && wget -O cerebro-${CEREBRO_VERSION}.tgz https://github.com/lmenezes/cerebro/releases/download/v${CEREBRO_VERSION}/cerebro-${CEREBRO_VERSION}.tgz \
    && tar zxvf cerebro-${CEREBRO_VERSION}.tgz \
    && rm cerebro-${CEREBRO_VERSION}.tgz \
    && mkdir cerebro-${CEREBRO_VERSION}/logs \
    && mv cerebro-${CEREBRO_VERSION} cerebro

COPY docker-entrypoint.sh /

WORKDIR /opt/cerebro
EXPOSE 9000
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["./bin/cerebro"]
