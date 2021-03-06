FROM docker:stable

ENV NODE_VERSION 6-alpine
ENV NODE_LOGLEVEL error

ADD https://github.com/titanium-codes/cibu/archive/master.zip /tmp/cibu.zip
ADD https://downloads.sentry-cdn.com/sentry-cli/1.44.4/sentry-cli-Linux-x86_64 /usr/local/bin/sentry-cli
RUN apk --no-cache add curl openssh-client git jq bash py-pip build-base openssl-dev libffi-dev python2-dev && \
    pip install docker-compose awscli s3cmd ansible && \
    printf "#!/bin/sh\ndocker run --rm -t -v \$PWD:/app --workdir /app -e NPM_CONFIG_LOGLEVEL=\$NODE_LOGLEVEL node:\$NODE_VERSION npm install \$@" > /usr/local/bin/npm-install && \
    printf "#!/bin/sh\ndocker run --rm -t -v \$PWD:/app --workdir /app -e NPM_CONFIG_LOGLEVEL=\$NODE_LOGLEVEL node:\$NODE_VERSION npm \$@" > /usr/local/bin/npm && \
    printf "#!/bin/sh\ndocker run --rm -v \$PWD:/app tico/composer install --no-dev --ignore-platform-reqs --no-ansi --no-interaction --no-progress --no-scripts -a -d /app" > /usr/local/bin/composer-install && \
    printf "#!/bin/sh\ndocker run --rm -v \$PWD:/app tico/php-cs-fixer \$@" > /usr/local/bin/php-cs-fixer && \
    printf "#!/bin/sh\ndocker run --rm -v \$PWD:/app tico/security-checker \$@" > /usr/local/bin/security-checker && \
    printf "#!/bin/sh\ndocker run --rm -v \$PWD:/app tico/unused-scanner \$@" > /usr/local/bin/unused-scanner && \
    unzip /tmp/cibu.zip -d /tmp && mkdir -p /opt/cibu && \
    mv /tmp/cibu-master/cibu/* /opt/cibu/ && mv /tmp/cibu-master/bin /usr/local/bin/cibu && \
    rm -rf /tmp/cibu* && chmod +x /usr/local/bin/*
