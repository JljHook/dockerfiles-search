FROM ruby:2.4

MAINTAINER thinkbot@outlook.de

ENV VERSION=0.4.2

RUN gem install codes --version ${VERSION} --no-format-exec

WORKDIR /tmp

ENTRYPOINT ["codes"]
CMD ["--help"]
