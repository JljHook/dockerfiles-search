FROM ubuntu:latest

# make Apt non-interactive
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y git locales openssh-client ca-certificates wget tzdata language-pack-pl python3 gettext-base

# uncomment chosen locale to enable it's generation
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
RUN sed -i -e 's/# pl_PL.UTF-8 UTF-8/pl_PL.UTF-8 UTF-8/' /etc/locale.gen

# generate chosen locale
RUN locale-gen pl_PL.UTF-8
RUN locale-gen en_US.UTF-8

# set system-wide locale settings
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US

# verify modified configuration
RUN dpkg-reconfigure --frontend noninteractive locales

CMD ["/bin/sh"]
