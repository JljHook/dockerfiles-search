FROM prima/elixir-official:1.9.1-1

COPY files/apt/99defaultrelease /etc/apt/apt.conf.d/99defaultrelease
COPY files/apt/stable.list /etc/apt/sources.list.d/stable.list
COPY files/apt/testing.list /etc/apt/sources.list.d/testing.list
COPY files/cron-magick /tmp/cron-magick

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main 9.6" > "/etc/apt/sources.list.d/postgresql.list" && \
    curl -sL https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    apt-get update && \
    apt-get install -qqy postgresql-client-9.6 apt-utils && \
    apt-get upgrade -y && \
    apt-get -t testing install -qqy ghostscript imagemagick && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

RUN	apt-get update && \
	apt-get install -y python2.7 python-pip cron && \
	pip install awscli && \
    aws configure set s3.signature_version s3v4 && \
    crontab /tmp/cron-magick && \
    rm -f /tmp/cron-magick

COPY files/entrypoint.sh /entrypoint.sh

CMD ["/entrypoint.sh"]
