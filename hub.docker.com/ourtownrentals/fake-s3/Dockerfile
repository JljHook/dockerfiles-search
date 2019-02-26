FROM ruby:2.3-alpine

MAINTAINER Evan Sosenko <razorx@evansosenko.com>

RUN mkdir -p /srv

ENV FAKES3_VERSION 2.0.0

RUN gem install --no-document fakes3 -v ${FAKES3_VERSION}

WORKDIR /srv

VOLUME /srv
EXPOSE 80

ENTRYPOINT ["fakes3", "-r", "/srv", "-p", "80"]
