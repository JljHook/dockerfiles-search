FROM eeacms/node:v4.2.2-1.1
MAINTAINER "European Environment Agency (EEA): IDM2 A-Team" <eea-edw-a-team-alerts@googlegroups.com>
ENV NODE_ENV 'production'
ADD ./README.md /tmp/README.md
ADD ./app/package.json /tmp/package.json
RUN cd /tmp && npm install && mv /tmp/node_modules /node_modules
ADD . /sources_from_git
RUN ln -s /sources_from_git/app /code
VOLUME /code/app/config
USER node
