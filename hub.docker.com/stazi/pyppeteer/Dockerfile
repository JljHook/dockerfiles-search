
FROM python:3.6
LABEL maintainer="stazi@fruitsoftware.com"

# download chromium so some download chrome libs and deps
# Note: might not be needed. pypeteer download is giving me:
# pyppeteer.errors.BrowserError: Failed to connect to browser port: http://127.0.0.1:36853/json/version
RUN apt-get update && apt-get install -y wget --no-install-recommends \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-unstable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst ttf-freefont \
      --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge --auto-remove -y curl \
    && rm -rf /src/*.deb

# install pyppeteer
RUN pip install git+git://github.com/miyakogi/pyppeteer.git@dev

# let pypeteer downloads to download chromium (needed)
RUN python -c "from pyppeteer.chromium_downloader import download_chromium; download_chromium()"
