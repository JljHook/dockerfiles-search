FROM node:12-alpine

ARG NPM_REGISTRY=https://registry.npmjs.org

ARG SMEE_VERSION=1.1.0

ENV SMEE_VERSION=${SMEE_VERSION}

RUN npm install --global smee-client@${SMEE_VERSION} \
      --registry=${NPM_REGISTRY} \
    && rm -rf /root/.npm

ENTRYPOINT ["smee"]

CMD ["--help"]
