FROM redis:alpine

RUN apk --no-cache add drill

COPY entrypoint.sh /usr/local/bin/

RUN chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["entrypoint.sh"]
