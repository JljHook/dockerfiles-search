FROM node:9-alpine

RUN apk update && apk upgrade && \
    echo @edge http://nl.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories && \
    echo @edge http://nl.alpinelinux.org/alpine/edge/main >> /etc/apk/repositories && \
    apk add --no-cache \
      bash

RUN npm install pm2 -g

COPY . /app/
WORKDIR app

RUN npm install

ENV PORT 3000
EXPOSE 3000

#CMD ["node", "server.js"]
CMD ["pm2-runtime", "pm2.yaml"]
