FROM ubuntu:14.04
MAINTAINER Oto Simecek <aranwes@gmail.com>

ENV POSTGREST_VERSION 0.4.1.0
ENV POSTGREST_DBHOST db_host
ENV POSTGREST_DBPORT 5432
ENV POSTGREST_DBNAME db_name
ENV POSTGREST_DBUSER db_user
ENV POSTGREST_DBPASS db_pass
ENV POSTGREST_SCHEMA public
ENV POSTGREST_ANONYMOUS anon
ENV POSTGREST_JWT_SECRET secret
ENV POSTGREST_MAX_ROWS 1000000
ENV POSTGREST_POOL 200
ENV POSTGREST_PRE_REQUEST pre_request

RUN apt-get update && \
    apt-get install -y tar xz-utils wget libpq-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN wget http://github.com/begriffs/postgrest/releases/download/v${POSTGREST_VERSION}/postgrest-${POSTGREST_VERSION}-ubuntu.tar.xz && \
    tar --xz -xvf postgrest-${POSTGREST_VERSION}-ubuntu.tar.xz && \
    mv postgrest /usr/local/bin/postgrest && \
    rm postgrest-${POSTGREST_VERSION}-ubuntu.tar.xz

RUN mkdir /etc/postgrest

ADD build_and_run.sh /etc/postgrest

CMD ["sh", "-c", "/etc/postgrest/build_and_run.sh"]

EXPOSE 3000
