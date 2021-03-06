FROM ubuntu:18.10
MAINTAINER VCA Technology <developers@vcatechnology.com>

# Build-time metadata as defined at http://label-schema.org
ARG PROJECT_NAME
ARG BUILD_DATE
ARG VCS_REF
ARG VERSION
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.name="$PROJECT_NAME" \
      org.label-schema.description="A Ubuntu image that is updated daily to provide the latest packages" \
      org.label-schema.url="https://www.ubuntu.com/" \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url="https://github.com/vcatechnology/docker-ubuntu" \
      org.label-schema.vendor="VCA Technology" \
      org.label-schema.version=$VERSION \
      org.label-schema.license=MIT \
      org.label-schema.schema-version="1.0"

# Make sure APT operations are non-interactive
ENV DEBIAN_FRONTEND noninteractive

# Create install script
RUN touch                                                                 /usr/local/bin/vca-install-package \
 && chmod +x                                                              /usr/local/bin/vca-install-package \
 && echo '#! /bin/sh'                                                  >> /usr/local/bin/vca-install-package \
 && echo 'set -e'                                                      >> /usr/local/bin/vca-install-package \
 && echo 'apt-get -q update'                                           >> /usr/local/bin/vca-install-package \
 && echo 'apt-get -qy -o Dpkg::Options::="--force-confnew" install $@' >> /usr/local/bin/vca-install-package \
 && echo 'apt-get -qy clean'                                           >> /usr/local/bin/vca-install-package

# Create uninstall script
RUN touch                                   /usr/local/bin/vca-uninstall-package \
 && chmod +x                                /usr/local/bin/vca-uninstall-package \
 && echo '#! /bin/sh'                    >> /usr/local/bin/vca-uninstall-package \
 && echo 'set -e'                        >> /usr/local/bin/vca-uninstall-package \
 && echo 'apt-get -qy remove --purge $@' >> /usr/local/bin/vca-uninstall-package \
 && echo 'apt-get -qy autoremove'        >> /usr/local/bin/vca-uninstall-package \
 && echo 'apt-get -qy clean'             >> /usr/local/bin/vca-uninstall-package

# Generate locales
RUN vca-install-package apt-utils \
 && vca-install-package locales language-pack-en \
 && echo "LANG=en_GB.UTF-8" > /etc/default/locale \
 && update-locale LANG=en_GB.UTF-8
ENV LANG=en_GB.UTF-8

# Set up the timezone
RUN vca-install-package tzdata \
 && echo "Europe/London" > /etc/timezone \
 && dpkg-reconfigure tzdata

# Update all packages
RUN apt-get -q update \
 && apt-get -qy -o Dpkg::Options::="--force-confnew" dist-upgrade \
 && apt-get -qy autoremove \
 && apt-get -q clean
