FROM clusio/meteord:node-4.8.6-base
MAINTAINER MeteorHacks Pvt Ltd.

ARG NODE_VERSION
ENV NODE_VERSION ${NODE_VERSION:-4.8.6}
ONBUILD ENV NODE_VERSION ${NODE_VERSION:-4.8.6}

ONBUILD COPY ./ /app
ONBUILD RUN bash $METEORD_DIR/lib/install_meteor.sh
ONBUILD RUN bash $METEORD_DIR/lib/build_app.sh
