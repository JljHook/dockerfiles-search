FROM ruby:2.4

MAINTAINER thinkbot@outlook.de

ENV VERSION=0.2.0

RUN gem install dbdump_command --version ${VERSION} --no-format-exec

WORKDIR /tmp

ENTRYPOINT ["dbdump"]
CMD ["--help"]
