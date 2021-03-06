#
# Notifier docker image
#
# http://github.com/tenstartups/notifier-docker
#

FROM tenstartups/alpine:latest

LABEL maintainer="Marc Lennox <marc.lennox@gmail.com>"

# Set environment variables.
ENV \
  TERM=xterm-color

# Install base packages.
RUN \
  apk --update add bash build-base busybox-extras ca-certificates coreutils curl nano procps rsync ruby ruby-dev ruby-json tar wget && \
  update-ca-certificates && \
  rm -rf /var/cache/apk/*

# Install ruby gems.
RUN gem install colorize hipchat rest-client slack-notifier --no-document

# Add files to the container.
COPY entrypoint.sh /docker-entrypoint
ADD script /tmp/script

# Copy scripts and configuration into place.
RUN \
  find /tmp/script -type f -name '*.sh' | while read f; do cp -v "$f" "/usr/local/bin/`basename ${f%.sh}`"; done && \
  find /tmp/script -type f -name '*.rb' | while read f; do cp -v "$f" "/usr/local/bin/`basename ${f%.rb}`"; done && \
  rm -rf /tmp/script

# Define entrypoint script.
ENTRYPOINT ["/docker-entrypoint"]

# Define default command.
CMD ["/usr/local/bin/notify"]
