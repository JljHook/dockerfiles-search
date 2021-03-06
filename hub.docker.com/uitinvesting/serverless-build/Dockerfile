FROM circleci/python:3.7

#
# Node
#

# gpg keys listed at https://github.com/nodejs/node#release-team
RUN set -ex \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys 4ED778F539E3634C779C87C6D7062848A1AB005C \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys B9E2F5981AA6E0CD28160D9FF13993A75599653C \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys 94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys B9AE9905FFD7803F25714661B63B535A4C206CA9 \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys 77984A986EBC2AA786BC0F66B01FBB92821C587A \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys 71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys FD3A5288F042B6850C66B31F09FE44734EB7990E \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys 8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys A48C2BEE680E841632CD4E44F07496B3EB3C1762

ENV NPM_CONFIG_LOGLEVEL info
ENV NODE_VERSION 10.15.3

RUN ARCH= && dpkgArch="$(dpkg --print-architecture)" \
  && case "${dpkgArch##*-}" in \
    amd64) ARCH='x64';; \
    ppc64el) ARCH='ppc64le';; \
    *) echo "unsupported architecture"; exit 1 ;; \
  esac \
  && cd /tmp \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-$ARCH.tar.xz" \
  && curl -SLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-$ARCH.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && sudo tar -xJf "node-v$NODE_VERSION-linux-$ARCH.tar.xz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-$ARCH.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && sudo ln -s /usr/local/bin/node /usr/local/bin/nodejs

#
# Yarn
#
RUN curl -o- -L https://yarnpkg.com/install.sh | bash
ENV PATH "$PATH:/home/circleci/.yarn/bin"

#
# Pipenv
#
RUN sudo pip install --upgrade --no-cache-dir pip
RUN sudo pip install --no-cache-dir \
  black \
  coverage \
  pipenv

#
# Java and DynamoDB local
#
RUN sudo apt-get update && sudo apt-get install -y \
    default-jre \
 && sudo rm -rf /var/lib/apt/lists/*

RUN cd /tmp \
  && curl -SLO https://s3-us-west-2.amazonaws.com/dynamodb-local/dynamodb_local_latest.tar.gz \
  && sudo mkdir -p /usr/local/dynamodb \
  && sudo tar -zxf dynamodb_local_latest.tar.gz -C /usr/local/dynamodb \
  && rm dynamodb_local_latest.tar.gz 

