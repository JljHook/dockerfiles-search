FROM node:8.8.0-slim

# Install latest chrome package.
# Note: this installs the necessary libs to make the bundled version of Chromium that Pupppeteer
# installs, work.
RUN apt-get update && apt-get install -y wget --no-install-recommends \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo ""deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main"" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-stable \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge --auto-remove -y curl \
    && rm -rf /src/*.deb

RUN apt-get update && apt-get install -y xvfb xauth \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /src/*.deb

RUN mkdir -p /app

RUN cd /app && echo "{}" > package.json && npm install --save puppeteer@1.6.0

