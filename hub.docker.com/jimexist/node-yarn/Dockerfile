FROM node:6

ADD https://yarnpkg.com/install.sh /tmp/install-yarn.sh

RUN chmod +x /tmp/install-yarn.sh && \
    /bin/bash /tmp/install-yarn.sh && \
    rm /tmp/install-yarn.sh && \
    ln -s /root/.yarn/bin/yarn /usr/local/bin/yarn
