FROM ubuntu:trusty-20170119

MAINTAINER purewind "qingfengbaozai@gmail.com"  

ADD sources.list /etc/apt

RUN echo 'APT::Install-Recommends 0;' >> /etc/apt/apt.conf.d/01norecommends \
 && echo 'APT::Install-Suggests 0;' >> /etc/apt/apt.conf.d/01norecommends \
 && apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y vim.tiny wget sudo net-tools ca-certificates unzip apt-transport-https \
 && rm -rf /var/lib/apt/lists/*