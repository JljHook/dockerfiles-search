FROM node:12-alpine

WORKDIR /data/app

RUN apk --no-cache --no-progress update && \
    apk --no-cache --no-progress upgrade && \
    # Install tini supervisor
    apk --no-cache --no-progress add tini bash git && \
    # add build tools
    apk --no-cache --no-progress add --virtual .build-deps python2 && \
  # Update NPM
    npm install -g npm && \
    apk --no-cache --no-progress add yarn && \
    # Install Angular.io
    yarn global add @angular/cli && \
    yarn add @angular-devkit/build-angular -d

# Add start script.
ADD entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT [ "/sbin/tini", "--", "/entrypoint.sh" ]