FROM alpine:3.3
MAINTAINER Robin Naundorf <r.naundorf@fh-muenster.de>
RUN apk add --update unrar && rm -rf /var/cache/apk/* 
VOLUME /work
WORKDIR /work
ENTRYPOINT ["unrar"]
