FROM ocelotuproar/alpine-node:5.7.1

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY package.json /usr/src/app/
RUN npm install

COPY . /usr/src/app

ENV REDIS_SERVER redis

EXPOSE 3000
CMD [ "npm", "start" ]
