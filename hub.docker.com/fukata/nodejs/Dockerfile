FROM node:10.15-stretch

ENV LANG C.UTF-8

RUN npm install -g npm && \
    npm install -g nodemon && \
    npm isntall -g http-server && \
    npm install -g live-server

ENV ENTRYKIT_VERSION 0.4.0
RUN wget https://github.com/progrium/entrykit/releases/download/v${ENTRYKIT_VERSION}/entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
    && tar -xvzf entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
    && rm entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
    && mv entrykit /bin/entrykit \
    && chmod +x /bin/entrykit \
    && entrykit --symlink

RUN mkdir /app

WORKDIR /app

ENTRYPOINT [ \
  "prehook", "node -v", "--"]
