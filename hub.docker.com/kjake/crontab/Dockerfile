FROM library/docker:stable

ENV HOME_DIR=/opt/crontab
RUN apk add --no-cache --virtual .run-deps bash jq openssh tzdata \
    && mkdir -p ${HOME_DIR}/jobs ${HOME_DIR}/projects \
    && adduser -S docker -D \
    && chown cron:cron /var/spool/cron

COPY docker-entrypoint /
ENTRYPOINT ["/docker-entrypoint"]

HEALTHCHECK --interval=5s --timeout=3s \
    CMD ps aux | grep '[c]rond' || exit 1

CMD ["crond","-f"]
