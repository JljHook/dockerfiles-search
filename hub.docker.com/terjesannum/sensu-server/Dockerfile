FROM terjesannum/sensu-base:2
LABEL maintainer "terje@offpiste.org"

ENV LOG_LEVEL=info

ENV SENSU_HOST=sensu-api
ENV SENSU_PORT=4567

ENV REDIS_HOST=redis
ENV REDIS_PORT=6379

ENV INFLUXDB_HOST=influxdb
ENV INFLUXDB_PORT=8086
ENV INFLUXDB_DATABASE=metrics
ENV INFLUXDB_USER=sensu
ENV INFLUXDB_PASSWORD=sensu

ENV RABBITMQ_HOST=rabbitmq
ENV RABBITMQ_PORT=5672
ENV RABBITMQ_VHOST=/
ENV RABBITMQ_USER=guest
ENV RABBITMQ_PASSWORD=guest

ENV BUFFER_SIZE 1000
ENV BUFFER_MAX_AGE 10

RUN /opt/sensu/embedded/bin/gem install -v 2.1.0 sensu-extensions-influxdb

COPY run.sh /opt/
CMD ["/opt/run.sh"]
