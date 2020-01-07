FROM ubuntu:bionic
MAINTAINER ViViDboarder <vividboarder@gmail.com>

RUN apt-get update \
        && apt-get install -y --no-install-recommends \
            ca-certificates \
            cron \
            restic \
        && apt-get clean \
        && rm -rf /var/apt/lists/*

VOLUME /root/.cache/restic
VOLUME /backups

ENV BACKUP_DEST="/backups"
ENV BACKUP_NAME="backup"
ENV PATH_TO_BACKUP="/data"

# Cron schedules
ENV CRON_SCHEDULE=""
ENV VERIFY_CRON_SCHEDULE=""

COPY backup.sh /
COPY restore.sh /
COPY start.sh /
COPY verify.sh /
COPY healthcheck.sh /
COPY cron-exec.sh /

HEALTHCHECK CMD /healthcheck.sh

CMD [ "/start.sh" ]
