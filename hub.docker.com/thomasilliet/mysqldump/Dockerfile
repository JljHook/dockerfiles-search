FROM alpine:3.6
RUN apk add --update mysql-client bash openssh-client && rm -rf /var/cache/apk/*
COPY dump.sh /
RUN chmod +x /dump.sh
ENTRYPOINT ["/dump.sh"]
