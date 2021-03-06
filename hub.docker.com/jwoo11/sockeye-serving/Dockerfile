FROM ubuntu:18.04

ENV PYTHONUNBUFFERED TRUE
ENV SOCKEYE_VERSION=1.18.97
ENV TEMP=/home/model-server/tmp

RUN useradd -m model-server && \
    mkdir -p $TEMP && \
    chown model-server $TEMP

WORKDIR /home/model-server

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
    build-essential \
    python3-dev \
    python3-venv \
    openjdk-8-jdk-headless \
    curl \
    vim && \
    rm -rf /var/lib/apt/lists/*

COPY requirements/ requirements/
RUN python3 -m venv venv && \
    . venv/bin/activate && \
    pip install --no-cache-dir -U pip setuptools wheel && \
    pip install --no-cache-dir sockeye==$SOCKEYE_VERSION && \
    pip install --no-cache-dir -r requirements/sockeye-serving/requirements.txt

COPY config/config.properties .
COPY docker/dockerd-entrypoint.sh /usr/local/bin/dockerd-entrypoint.sh

EXPOSE 8080 8081 8443 8444

USER model-server
ENTRYPOINT ["/usr/local/bin/dockerd-entrypoint.sh"]
CMD ["serve"]

LABEL version="1.1.0" \
      maintainer="james.e.woo@gmail.com" \
      source="https://github.com/jamesewoo/sockeye-serving" \
      description="A containerized service for neural machine translation"
