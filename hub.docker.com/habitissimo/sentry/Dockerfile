FROM python:2.7

MAINTAINER Sławek Ehlert <slafs@op.pl>

RUN pip install -U wheel pip setuptools

RUN apt-get -qq update && DEBIAN_FRONTEND=noninteractive apt-get install -y -q libxslt1-dev libxml2-dev libpq-dev libldap2-dev libsasl2-dev libssl-dev && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /conf /data /wheels

ADD requirements.txt /conf/

RUN pip wheel --wheel-dir=/wheels -r /conf/requirements.txt && pip install --find-links=/wheels -r /conf/requirements.txt && rm -rf /wheels/*

EXPOSE 9000

VOLUME ["/data"]

ADD sentry_docker_conf.py /conf/
ADD sentry_run /usr/local/bin/

ENTRYPOINT ["/usr/local/bin/sentry_run"]

CMD ["run", "web"]

ADD scripts/create_team_or_project.py /conf/
ADD scripts/check_db_isalive.py /conf/


