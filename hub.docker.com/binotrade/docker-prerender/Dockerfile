FROM node:8.9

EXPOSE 3000

# Install dumb-init to rape any Chrome zombies
RUN wget https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64.deb
RUN dpkg -i dumb-init_*.deb

# Install Chromium.
RUN \
  wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
  apt-get update && \
  apt-get install -y google-chrome-stable && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*
RUN google-chrome-stable --no-sandbox --version > /opt/chromeVersion

RUN mkdir -p /usr/src/app
RUN groupadd -r prerender && useradd -r -g prerender -d /usr/src/app prerender
RUN chown prerender:prerender /usr/src/app

USER prerender
WORKDIR /usr/src/app

ADD yarn.lock /usr/src/app/
ADD package.json /usr/src/app/

USER root
RUN chmod 0777 /usr/src/app/yarn.lock

USER prerender
RUN yarn install
COPY . /usr/src/app

CMD [ "dumb-init", "yarn", "prod" ]
