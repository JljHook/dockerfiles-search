FROM ruby:2.4

MAINTAINER thinkbot@outlook.de

ENV VERSION=1.2.9

RUN gem install asir --version ${VERSION} --no-format-exec

WORKDIR /tmp

ENTRYPOINT ["asir"]
CMD ["--help"]
