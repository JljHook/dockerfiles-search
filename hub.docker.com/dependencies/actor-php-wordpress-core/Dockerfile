FROM python:3.6-alpine

RUN apk --no-cache add git curl wget

# add a non-root user and give them ownership
RUN adduser -D -u 9000 app && \
    # repo
    mkdir /repo && \
    chown -R app:app /repo && \
    # actor code
    mkdir /usr/src/actor && \
    chown -R app:app /usr/src/actor

# add the pullrequest utility to easily create pull requests on different git hosts
WORKDIR /usr/src/actor
ENV PULLREQUEST_VERSION=0.6.0
RUN wget https://github.com/dependencies-io/deps/releases/download/${PULLREQUEST_VERSION}/pullrequest_${PULLREQUEST_VERSION}_linux_amd64.tar.gz && \
    mkdir pullrequest && \
    tar -zxvf pullrequest_${PULLREQUEST_VERSION}_linux_amd64.tar.gz -C pullrequest && \
    ln -s /usr/src/actor/pullrequest/pullrequest /usr/local/bin/pullrequest

# run everything from here on as non-root
USER app

RUN git config --global user.email "bot@dependencies.io"
RUN git config --global user.name "Dependencies.io Bot"

ADD entrypoint.py /usr/src/actor

WORKDIR /repo

ENTRYPOINT ["python", "/usr/src/actor/entrypoint.py"]
