FROM node:8.2.1-alpine
MAINTAINER binking <binking338@163.com>

#------------------------------------------------------------------------------
# Environment variables:
#------------------------------------------------------------------------------
ENV PORT   3000

#------------------------------------------------------------------------------
# Expose ports and entrypoint:
#------------------------------------------------------------------------------
EXPOSE $PORT

VOLUME ["/usr/src/app"]
WORKDIR /usr/src/app

CMD ["app.js"]
ENTRYPOINT ["node"]
